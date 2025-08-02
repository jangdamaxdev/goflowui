# class ComfyApi extends EventTarget

this.api_base là địa chỉ server backend, mặc định là localhost:8188.
#registered = new Set()

## internalURL(route: string): string

Trả về địa chỉ `IPserver/internal + route`

## apiURL(route: string): string

Trả về địa chỉ: `IPserver/api + route`. Thường dùng bên trong lệnh gọi this.fetchAPI

## fileURL(route: string): string

Trả về địa chỉ: `IPserver + route`. Thường dùng cho lấy file thông qua Axios.

## fetchApi(route: string, options?: RequestInit)

- Tạo Object: Options, Headers rỗng nếu chưa có.
- Nếu Options, Headers đã có thì thêm tên this.user vào (dùng cho trường hợp có tài khoản ComfyUI). Mặc định this.user là `'Jang'`

```ts
fetchApi(route: string, options?: RequestInit) {
    if (!options) {
      options = {}
    }
    if (!options.headers) {
      options.headers = {}
    }
    if (!options.cache) {
      options.cache = 'no-cache'
    }

    if (Array.isArray(options.headers)) {
      options.headers.push(['Comfy-User', this.user])
    } else if (options.headers instanceof Headers) {
      options.headers.set('Comfy-User', this.user)
    } else {
      options.headers['Comfy-User'] = this.user
    }
    console.log("Route:", route, "with user: ", this.user, "option bao gồm headers: ", options)
    return fetch(this.apiURL(route), options)
  }
```

## override addEventListener

Appends an event listener for events whose type attribute value is type. The callback argument sets the callback that will be invoked when the event is dispatched.
The options argument sets listener-specific options. For compatibility this can be a boolean, in which case the method behaves exactly as if the value was specified as options's capture.
When set to true, options's capture prevents callback from being invoked when the event's eventPhase attribute value is BUBBLING_PHASE. When false (or not present), callback will not be invoked when event's eventPhase attribute value is CAPTURING_PHASE. Either way, callback will be invoked if event's eventPhase attribute value is AT_TARGET

```ts
override addEventListener<TEvent extends keyof ApiEvents>(
    type: TEvent,
    callback: ((event: ApiEvents[TEvent]) => void) | null,
    options?: AddEventListenerOptions | boolean
  ) {
    // Type assertion: strictFunctionTypes.  So long as we emit events in a type-safe fashion, this is safe.
    super.addEventListener(type, callback as EventListener, options)
    this.#registered.add(type)
  }
```

- `TEvent extends keyof ApiEvents` Chỉ cho phép type là các key hợp lệ trong ApiEvents. Giúp type-safe. Nguyên mẫu `type: string`dễ viết nhầm tên sự kiện.
- Kiểu event trong `callback` phụ thuộc vào `type: TEvent`. Giúp IntelliSense và tự động bắt lỗi.
- `options?: ...` Vẫn hỗ trợ đầy đủ capture, once, passive như chuẩn DOM.
- `super.addEventListener(...)` Gọi API gốc của EventTarget.
- `this.#registered.add(type)`Lưu lại sự kiện đã đăng ký. Có thể dùng sau này để quản lý/bỏ đăng ký.

## override removeEventListener

_Removes the event listener in target's event listener list with the same type, callback, and options._
Bạn đang ghi đè (override) phương thức chuẩn removeEventListener() trong DOM's EventTarget.
Tương tự cách vận hành như hàm override `addEventListener` ở trên.

```ts
override removeEventListener<TEvent extends keyof ApiEvents>(
    type: TEvent,
    callback: ((event: ApiEvents[TEvent]) => void) | null,
    options?: EventListenerOptions | boolean
  ): void {
    super.removeEventListener(type, callback as EventListener, options)
  }
```

Kiểu tham số type giờ đã được giới hạn vào danh sách key của ApiEvents → `đảm bảo chỉ gọi với tên sự kiện hợp lệ.`
=> Không thấy tương tác với `this.#registered` như trong hàm `addEventListener` ở trên. Ví dụ add rồi remove xong phải gỡ ra.

## dispatchCustomEvent<T>: khai báo 2 kiểu method overload

khai báo hàm cùng tên: dispatchCustomEvent, nhưng có kiểu tham số khác nhau:

- Overload 1: (type: `SimpleApiEvents`): boolean:
  với type = `"graphCleared" | "reconnecting" | "reconnected"`
- Overload 2: (type: `ComplexApiEvents`, detail: ApiEventTypes[T] | null): boolean
  với type = `"progress" | "executing" | "executed" | "status" | "execution_start" | "execution_success" | "execution_error" | "execution_interrupted" | "execution_cached" | "logs" | "b_preview" | ... 6 more ... | "promptQueued"`
  Lưu ý: Overload 2 detail là bắt buộc, nên khi dùng method này mà có detail thì nó vô overload 2.

## Thân hàm logic cho dispatchCustomEvent ở trên

Kiểm tra detail để thực hiện hàm cho hướng 1 hay 2.
Đây là thân hàm duy nhất dùng cho 2 kiểu khai báo ở trên, nếu có nhiều khai báo kiểu như trên thì thân hàm này phải kiểm tra các tham số và đi flow cho phù hợp.
Thường dùng kiểu này cho các hàm khác nhau chút về số lượng, kiểu tham số, dễ quản lý

```ts
dispatchCustomEvent<T extends keyof ApiEventTypes>(
    type: T,
    detail?: ApiEventTypes[T]
  ): boolean {
    const event =
      detail === undefined
        ? new CustomEvent(type)
        : new CustomEvent(type, { detail })
      console.log("gọi phương thức dispatchCustomEvent với event: ", event)
    return super.dispatchEvent(event)
  }
```

## (method) ComfyApi.dispatchEvent

_Dispatches a synthetic event event to target and returns true if either event's cancelable attribute value is false or its preventDefault() method was not invoked, and false otherwise._

```ts
override dispatchEvent(event: never): boolean {
    return super.dispatchEvent(event)
  }
```

Là một method override trong TypeScript/JavaScript class (rất có thể là một class kế thừa từ `EventTarget`), nhưng có một điểm rất đặc biệt: tham số event có kiểu là `never`

- Đây là ghi đè (override) phương thức dispatchEvent() gốc, vốn có trong DOM API
- `never` là kiểu TypeScript biểu thị rằng giá trị không bao giờ tồn tại được. Một hàm nhận tham số kiểu never là không thể nào được gọi hợp lệ nếu bạn tuân thủ kiểm tra kiểu TypeScript. Vậy tức là: hàm này bị "khóa lại" để không thể được dùng một cách bình thường.
- Hàm vẫn gọi `super.dispatchEvent(...)`, tức là vẫn thực hiện logic mặc định nếu somehow có event hợp lệ (về runtime). Nhưng nếu TypeScript đang được dùng nghiêm túc, bạn sẽ không thể gọi được hàm này, vì không bao giờ có biến nào là `never`

## hàm async queuePrompt

```ts
async queuePrompt(
    number: number,
    data: { output: ComfyApiWorkflow; workflow: ComfyWorkflowJSON },
    options?: QueuePromptOptions
  ): Promise<PromptResponse>
```

### truy xuất dữ liệu từ data: const { output: prompt, workflow } = data

#### prompt

```ts
// thường được gọi là prompt
type ComfyApiWorkflow = {
    [x: string]: {
        inputs: Record<string, any>;
        class_type: string;
        _meta: {
            title: string;
        };
    };
    [x: number]: {
        inputs: Record<string, any>;
        class_type: string;
        _meta: {
            title: string;
        };
    };
}
import ComfyApiWorkflow
```

#### workflow

```ts
// thường được gọi là workflow
(alias) type ComfyWorkflowJSON = objectOutputType<objectUtil.extendShape<{
    id: ZodOptional<ZodString>;
    revision: ZodOptional<ZodNumber>;
    config: ZodNullable<ZodOptional<ZodObject<{
        links_ontop: ZodOptional<ZodBoolean>;
        align_to_grid: ZodOptional<...>;
    }, "passthrough", ZodTypeAny, objectOutputType<...>, objectInputType<...>>>>;
    subgraphs: ZodOptional<...>;
}, {
    ...;
}>, ZodTypeAny, "passthrough"> | objectOutputType<...>
import ComfyWorkflowJSON
```

## kết quả hàm tạo COnstructor
Hàm tạo dựa trên giá trị của location. Với location của global:
```json
{
  "ancestorOrigins": {},
  "href": "http://localhost:5173/",
  "origin": "http://localhost:5173",
  "protocol": "http:",
  "host": "localhost:5173",
  "hostname": "localhost",
  "port": "5173",
  "pathname": "/",
  "search": "",
  "hash": ""
}
```
Giá trị location xuất phát từ truy cập của người dùng vào thanh URL của browser.
Để biết địa chỉ này thì do Vite tạo ra với lệnh `npm run dev`.
Muốn thay đổi thì thêm flag để thay đổi, ví dụ như: `npm run dev -- --host localhost --port 1234`
Nếu người dùng nhập `http://localhost:1234/abc/1223/` thì location sẽ là:
```json
{
  "ancestorOrigins": {},
  "href": "http://localhost:1234/abc/1223/",
  "origin": "http://localhost:1234",
  "protocol": "http:",
  "host": "localhost:1234",
  "hostname": "localhost",
  "port": "1234",
  "pathname": "/abc/1223/",
  "search": "",
  "hash": ""
}
```
`this.api_base` được xác định là `/abc/1223/` và trang web không hiển thị được. 
Thậm chí với `pathname: /abc/` (có dấu `/`) thì `this.api_base` cho ra `/abc`.
Tiếp tục xóa bớt `/` thì `pathname: /abc`. lÚc này sau khi split. `this.api_base =""` và mới vào được GUI.
Tóm lại: truy cập `http://localhost:1234` hoặc `http://localhost:1234/abc` (không có `/`) thì vào được GUI. 
Khi `this.api_base =""` rỗng thì các lệnh fetchApi được bắt đầu từ `"origin": "http://localhost:1234",` không có sub. 
### Kết quả tạo api instance

```json
{
  "api_host": "localhost:5173",
  "api_base": "",
  "initialClientId": "616993e5479941a5a4a6fa59f5af68a8",
  "user": "Jang",
  "socket": null,
  "reportedUnknownMessageTypes": {},
  "serverFeatureFlags": {}
}
```


### #Socket

````ts
socket:
WebSocket
binaryType
:
"arraybuffer"
bufferedAmount
:
0
extensions
:
""
onclose
:
null
onerror
:
null
onmessage
:
null
onopen
:
null
protocol
:
""
readyState
:
0
url
:
"ws://localhost:5173/ws"
[[Prototype]]
:
WebSocket
user
:
"Jang"```
````
