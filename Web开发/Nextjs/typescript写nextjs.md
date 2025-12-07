## 监听KeyboardEvent事件
```ts
export default function Home() {
  return (
    <div>
      <div
        contentEditable="true"
		placeholder="请输入内容..."
		onKeyDown={handle} // {} 中的函数必须是同步函数
		className="h-24 w-24"
      ></div>
    </div>
  );
}

// 在 js 中，可以使用同步函数包裹一个异步函数
function handle(event: React.KeyboardEvent<HTMLDivElement>) {
  ok(event);
}

async function ok(event: React.KeyboardEvent<HTMLDivElement>) {
    event.preventDefault();
    console.log("KeyboardEvent");
}
```