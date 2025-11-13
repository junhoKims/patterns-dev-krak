# Presentaion Container Design

React에서 컴포넌트의 UI와 로직, 사이드이펙트를 구분하여 구성하기 위한 기본적인 디자인 패턴

- UI에만 집중하는 Presentaional Component
- 로직과 사이드이펙트를 구성하는 집중하는 Custom Hook
- UI와 hook을 담아내는 Container Component

## 장점

- 단일 목적을 토대로 명확하게 분리되어 있어 높은 가독성을 보유
- 단위테스트와 통합테스트가 쉬워짐
- 추후 확장이 단순해짐

### Presentational Components

안에는 아래의 내용만 구성하는 것에 집중합니다.

- 순수 UI
- 다양한 스타일

```tsx title="UserCard.tsx"
function UserCard({ name, onFollow }) {
  return (
    <div className="card">
      <p>{name}</p>
      <button onClick={onFollow}>Follow</button>
    </div>
  );
}
```

### Custom Hook

hook 안에는 단일 기능(로직)을 담는 것에 집중합니다.

- 복합적인 기능을 분리할 수 있다면 분리하여 여러개의 hook으로 구성
- 사이드이펙트를 분리할 수 있다면 분리하여 구성

```tsx title="useUserProfile.ts" {6}
function useUserProfile(userId) {
  const [profile, setProfile] = useState(null);
  const [loading, setLoading] = useState(true);

useEffect(() => {
    async function fetchProfile() {
      setLoading(true);
      const res = await fetch(`/api/users/${userId}`);
      const data = await res.json();
      setProfile(data);
      setLoading(false);
    }
    fetchProfile();
  }, [userId]);
  return { profile, loading };
}
```

### Container Component

단일 목적을 가지고 분리된 Presentational Component와 Custom hook을 담는 컴포넌트

- Container에서 hoc 또는 Context Provider를 감쌀 수 있습니다.
- 위의 구조를 토대로 확대해 나갑니다.

```tsx title="UserProfileContainer"
function UserProfileContainer({ userId }) {
  const { profile, loading } = useUserProfile(userId);

if (loading) return <LoadingSpinner />;
  return <UserCard name={profile.name} onFollow={() => alert('Followed!')} />;
}
```

## 결론

- `UserCard`는 스타일과 JSX로만 이루어져 있어 가독성이 높고 단순해진다.
- `UserCard` UI로만 되어있기에 UI테스트 구현이 쉬워진다.
- `useUserProfile`는 단일 기능만 작성되어 있기에 가독성이 높다.
- `UserProfileContainer`에서 Provider 또는 hoc의 위치를 고민하지 않게된다.
- `UserProfileContainer`에서 통합테스트를 시작하기 쉬워진다.