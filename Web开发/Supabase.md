- [基础CRUD操作](#基础CRUD操作)
	- [查询数据](#查询数据)
	- [插入数据](#插入数据)
	- [更新数据](#更新数据)
	- [删除数据](#删除数据)
- [用户认证](#用户认证)
	- [注册用户](#注册用户)
	- [登录用户](#登录用户)
	- [获取当前用户](#获取当前用户)
	- [退出登录](#退出登录)
- [实时订阅](#实时订阅)
- [文件存储](#文件存储)
	- [上传文件](#上传文件)
	- [下载文件](#下载文件)
	- [获取公开URL](#获取公开URL)
每个表要启用行级安全（RLS）

supabase.ts 文件：
```ts
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = 'https://hmkqifpsdwyvuwlgvghv.supabase.co';
const supabaseAnonKey = 'xxx';

export const supabase = createClient(supabaseUrl, supabaseAnonKey);
```
## 基础CRUD操作
## 查询数据
```ts
// 获取所有数据
const { data, error } = await supabase
  .from('peoper')
  .select('*')
console.log(data?.[0]);


// 带条件的查询
const { data, error } = await supabase
  .from('todos')
  .select('id, task, completed')
  .eq('completed', true)
  .order('created_at', { ascending: false })
```
## 插入数据
```ts
const { data, error } = await supabase
  .from('peoper')
  .insert([
    {
	  name: 'John',
      task: 'Learn Supabase',
      completed: false,
    }
  ])
  .select() // 添加此行以返回插入的数据
console.log(data?.[0]);
```
## 更新数据
```ts
const { data, error } = await supabase
  .from('peoper')
  .update({ name: "Jack" })
  .eq('id', 1)
  .select(); // 添加此行以返回更新后的数据
console.log(data?.[0]);
```
## 删除数据
```ts
const { data, error } = await supabase
  .from('peoper')
  .delete()
  .eq('id', 7);

if (error) {
  console.error("删除操作发生错误:", error);
}
```
## 用户认证
## 注册用户
```ts
// 注意邮箱必须是有效的，不然无法发验证码
const { data, error } = await supabase.auth.signUp({
  email: 'milkmili842@gmail.com',
  password: 'canbelie283.'
});
console.log(data?.user?.email);
```
## 登录用户
```ts
const { data, error } = await supabase.auth.signInWithPassword({
  email: 'leiyue159@gmail.com',
  password: '123456789'
});
console.log(data?.user?.email);
```
## 获取当前用户
```ts
const { data: { user } } = await supabase.auth.getUser();
console.log(user?.email);
```
## 退出登录
```ts
const { error } = await supabase.auth.signOut();

if (error) {
  console.error("删除操作发生错误:", error);
}
```
## 实时订阅
```ts
const subscription = supabase
  .channel('todos-changes')
  .on(
    'postgres_changes',
    {
      event: '*', // INSERT, UPDATE, DELETE
      schema: 'public',
      table: 'todos'
    },
    (payload) => {
      console.log('数据变化:', payload)
    }
  )
  .subscribe()
// 取消订阅
subscription.unsubscribe()
```
## 文件存储
## 上传文件
```ts
const { data, error } = await supabase.storage
  .from('avatars')
  .upload('user1-avatar.jpg', file)
```
## 下载文件
```ts
const { data, error } = await supabase.storage
  .from('avatars')
  .download('user1-avatar.jpg')
```
## 获取公开URL
```ts
const { data } = supabase.storage
  .from('avatars')
  .getPublicUrl('user1-avatar.jpg')
```
