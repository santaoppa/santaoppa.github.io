---
title: "[React] 게시판 프로젝트 - (8) UserContext를 활용해 로그인 상태 관리"
excerpt: "로그인된 사용자 정보를 전역 상태로 관리"

categories:
  - Web
tags:
  - [Web, Spring boot, React]

toc: true
toc_sticky: true

date: 2025-04-17
last_modified_at: 2025-04-17
---
## ‼️ 문제점
매번 API 요청을 통해 회원 정보를 불러오는 방식은 불필요한 반복 작업을 하고있었음.
  
### 기존 코드
```js
const accessToken = localStorage.getItem("accessToken");

const [user, setUser] = useState(null);

useEffect(() => {
    if (accessToken) {
        fetchUser().then((response) => {
            setUser(response);
        })
    } else {
        alert("로그인이 필요합니다.");
        navigate("/login");
    }
}, []);
```

## ☑️ 해결책
`UserContext`를 활용하여 로그인된 사용자 정보를 전역 상태로 관리하고, 이를 하위 컴포넌트에 context를 통해 전달함

### 1. `UserContext`를 생성
`UserContext`를 생성하여 로그인 상태를 관리할 수 있는 context를 정의함
```js
const UserContext = createContext();
```  

### 2. 사용자 정보와 로딩 상태를 반환하는 함수 생성
`UserContext`에서 제공하는 값(사용자 정보, 로딩 상태)을 하위 컴포넌트에서 사용할 수 있도록 반환하는 함수 `useAuth()`를 작성   

다른 컴포넌트에서 `useAuth()`만 호출하면 로그인된 사용자와 로딩 상태를 얻을 수 있음

```js
export const useAuth = () => {
    return useContext(UserContext);
};
```

### 3. UserProvider 컴포넌트 생성
`UserProvider`는 로그인 상태를 관리하고, 그 값을 `UserContext.Provider`를 통해 하위 컴포넌트에 전달함
  
```js
export const UserProvider = ({ children }) => {
    const [userInfo, setUserInfo] = useState(null);
    const [loading, setLoading] = useState(true);

    // 컴포넌트가 마운트될 때 한 번 실행
    useEffect(() => {
        const accessToken = localStorage.getItem("accessToken");

        if (accessToken) {
            fetchUser().then((response) => {
                setUserInfo(response);
                setLoading(false);
            }).catch(() => {
                setLoading(false);
                alert("로그인 상태가 아닙니다.");
                window.location.href = "/login";
            });
        } else {
            setLoading(false);
        }
    }, []);

    const value = {
        userInfo,
        loading,
    };

    // children을 감싸서 하위 컴포넌트들에서 userInfo와 loading을 사용할 수 있도록 제공
    return <UserContext.Provider value={value}>{children}</UserContext.Provider>;
};
```
  

### 4. `UserProvider` 사용범위 지정
`App` 컴포넌트에서 `UserProvider`로 전체 애플리케이션을 감싸면, 하위 컴포넌트에서 로그인 정보와 로딩 상태를 쉽게 사용할 수 있음
```js
function App() {
    return (
        <UserProvider>
            <Router>
                <div className="App">
                    <Navigation />
                    <Routes>
                        <Route path="/" element={<Home />} />
                        <Route path="/write" element={<BbsWrite />} />
                        <Route path="/post/:id" element={<BbsDetail />} />
                        <Route path="/post/edit/:id" element={<BbsEdit />} />
                        <Route path="/signup" element={<SignUp />} />
                        <Route path="/login" element={<Login />} />
                    </Routes>
                </div>
            </Router>
        </UserProvider>
    );
}
```

### 5. 하위 컴포넌트에서 `UserContext` 사용
하위 컴포넌트에서는 `useAuth()`을 사용하여 로그인된 사용자 정보와 로딩 상태를 가져올 수 있음

```js
const [user, setUser] = useState(null);
const {userInfo, loading} = useAuth();

useEffect(() => {
        if (loading) {
            return;
        }

        if (!userInfo) {
            alert("로그인이 필요합니다.");
            window.location.href = "/login";
        }else {
            setUser(userInfo);
        }
    }, [loading, userInfo]);
```
