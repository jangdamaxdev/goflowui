Bạn đang xem một store Pinia dùng trong Vue 3 app, định nghĩa bằng `defineStore` để quản lý thông tin người dùng. Store này được gọi là `useUserStore`, và đúng như bạn quan sát: **mọi component đều dùng nó** để kiểm tra trạng thái đăng nhập, lấy user, thực hiện login/logout...

Dưới đây là **giải thích đầy đủ, chi tiết từng phần**, cả về luồng logic lẫn cách store này tích hợp với các component.

---

## 🧱 **Tổng quan: Store này dùng để làm gì?**

`useUserStore` là một **global store** cho:

1. Quản lý danh sách người dùng (`users`)
2. Lưu người dùng hiện tại (`currentUserId`)
3. Quyết định logic như:

   * Server có nhiều user không? → `isMultiUserServer`
   * Có cần login không? → `needsLogin`
   * Đã load config từ server chưa? → `initialized`
4. Thực hiện hành động:

   * `initialize()` – gọi API để lấy cấu hình người dùng
   * `createUser()` – tạo user mới
   * `login()` / `logout()` – thao tác đăng nhập, đăng xuất

---

## 🔍 **Chi tiết từng phần tử**

### 1. `userConfig: ref<UserConfig | null>`

```ts
const userConfig = ref<UserConfig | null>(null)
```

* Lưu trữ **toàn bộ config** trả về từ `/api/users`
* Kiểu `UserConfig` (import từ `apiSchema.ts`)
* Nếu là server đa người dùng, thì `userConfig.value.users` sẽ tồn tại
* Được gán bởi `await api.getUserConfig()` trong `initialize()`

---

### 2. `currentUserId: ref<string | null>`

```ts
const currentUserId = ref<string | null>(null)
```

* Giữ ID của user hiện tại
* Được lấy từ `localStorage['Comfy.userId']` trong `initialize()`
* Gán khi `login()`, xóa khi `logout()`

---

### 3. `isMultiUserServer: computed`

```ts
const isMultiUserServer = computed(() =>
  userConfig.value && 'users' in userConfig.value
)
```

* `true` nếu server đang ở chế độ đa người dùng (có `users` trong config)
* Dùng để xác định có cần login hay không

---

### 4. `needsLogin: computed`

```ts
const needsLogin = computed(() =>
  !currentUserId.value && isMultiUserServer.value
)
```

* Nếu chưa có `currentUserId` mà lại là server nhiều người dùng → cần login
* Được dùng trong **router guard** để chuyển hướng sang `/user-select`

---

### 5. `users: computed<User[]>`

```ts
const users = computed<User[]>(() =>
  Object.entries(userConfig.value?.users ?? {}).map(([userId, username]) => ({
    userId,
    username
  }))
)
```

* Từ object `userConfig.value.users` (dạng `{ userId1: username1, ... }`)
* Trả về mảng `User[]` dễ dùng cho UI dropdown, select...

---

### 6. `currentUser: computed`

```ts
const currentUser = computed<User | null>(() =>
  users.value.find(u => u.userId === currentUserId.value) ?? null
)
```

* Trả về thông tin user hiện tại (nếu có)

---

### 7. `initialized: computed`

```ts
const initialized = computed(() => userConfig.value !== null)
```

* Đã gọi `initialize()` thành công chưa
* Dùng để kiểm soát thời điểm UI render → chờ init xong mới cho user chọn

---

## 🔄 **Các hành động (actions)**

### 🧩 `initialize()`

```ts
async function initialize() {
  userConfig.value = await api.getUserConfig()
  currentUserId.value = localStorage['Comfy.userId']
}
```

* Gọi API `/api/users` → gán `userConfig`
* Lấy `userId` cũ từ localStorage → gán lại

> ⚠️ Nếu `userConfig` không có `users`, thì app hiểu là **single user mode**, không cần login.

---

### 🧩 `createUser(username)`

* Gọi API tạo user mới
* Trả về `userId + username`

### 🧩 `login(user: { userId, username })`

* Gán `currentUserId`
* Ghi thông tin vào localStorage

### 🧩 `logout()`

* Xóa userId và username khỏi localStorage

---

## 👁️‍🗨️ `watchEffect`:

```ts
watchEffect(() => {
  if (isMultiUserServer.value && currentUserId.value) {
    api.user = currentUserId.value
  }
})
```

* Tự động đồng bộ `api.user = currentUserId` nếu có login
* `api.user` dùng để gửi theo headers hoặc query khi gọi các API sau này

---

## ✅ Tóm tắt sơ đồ luồng hoạt động

```
App hoặc Router
   ↓
await userStore.initialize()
   ↓
→ gọi API /api/users → gán userConfig
→ lấy Comfy.userId từ localStorage → gán currentUserId
   ↓
→ Nếu needsLogin = true → redirect đến /user-select
→ Nếu needsLogin = false → render GraphView
```

---

## 🧪 Gợi ý kiểm tra/trích xuất

* Muốn biết app đang ở chế độ **nhiều người dùng** hay không:

  ```ts
  const store = useUserStore()
  console.log(store.isMultiUserServer)
  ```

* Muốn biết user hiện tại:

  ```ts
  const user = store.currentUser
  ```

* Muốn bắt buộc đăng nhập:

  ```ts
  if (store.needsLogin) router.push('/user-select')
  ```

---

Nếu bạn muốn mình viết một component ví dụ hiển thị danh sách `users` hoặc `login()` vào store, mình có thể hỗ trợ thêm.
