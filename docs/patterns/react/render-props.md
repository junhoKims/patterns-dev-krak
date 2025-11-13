# Render Prop

React 컴포넌트 간에 코드를 공유하기 위해 함수 props를 이용하는 기술 패턴

- `render`라는 이름의 함수 prop으로 React Element를 리턴.
- 즉각적으로 변하는 데이터에 대한 동적인 UI를 그려야 하는 경우 유용하다.
- 대체적으로 특정 UI를 재사용하거나, 동적인 상태 또는 값에 의해 UI가 리렌더링 되어야 하는 경우 구현하면 좋다.

## 예제

### 좋은 예제

```tsx title="MouseBoard.tsx" {7,22}
export function Board() {
  return (
    <Mouse
      name="mouse"
      render={(x, y) => {
        const pos = convert(x, y);
        return <Component mouse={pos} />
      }}
    />
  )
}

function Mouse({ name, render }) {
  const [curx] = useState(0);
  const [cury] = useState(0);

  return (
    <div>
      <p>{name}</p>
      <div>
        {render(curx, cury)}
      </div>
    </div>
  )
}
```

- 마우스 위치에 따라 특별한 계산(`convert()`) 후 컴포넌트를 리렌더링 해주는 코드
- 위 경우에서 renderProps 패턴은 유용하다.

### 나쁜 예제

```tsx title="BadCase.tsx" {2,9}
function Title({ render }) {
  return render();
}

function Section() {
  const [text] = useState('');

  return (
    <Title render={() => <p>{text}</p>} />
  )
}
```

- 하위 컴포넌트인 `Title`에서 동적으로 변하는 상태 또는 값이 없다.
- 하위 컴포넌트에서 동적인 값를 콜백으로 넘겨줄 필요가 없다면, render prop을 사용하지 않는게 좋다.

**아래처럼 개선**

```tsx title="GoodCase.tsx" 
function Title({ children }) {
  return <p>{children}</p>
}

function Section() {
  const [text] = useState('');
  return <Title>{text}</Title>
}
```
