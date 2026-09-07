# 커플 도장판

매일 할 일을 마치면 서로 도장을 찍어주고, 모은 도장으로 각자 원하는 쿠폰을 교환하는 커플용 습관 트래커입니다.

로그인 없이 링크만 있으면 누구나 들어와서 쓸 수 있도록, 데이터는 [Firebase](https://firebase.google.com) Firestore(무료)에 실시간으로 저장됩니다.

## 처음 한 번, Firebase 연결하기 (5분)

이 저장소를 그대로 배포하면 "Firebase 설정이 필요해요" 화면만 보여요. 아래 순서대로 딱 한 번만 설정하면 그 다음부터는 링크만 공유하면 됩니다.

1. [Firebase 콘솔](https://console.firebase.google.com)에 접속해서 새 프로젝트를 만듭니다 (무료 Spark 요금제로 충분해요).
2. 프로젝트 개요 화면에서 웹 아이콘(`</>`)을 눌러 웹 앱을 등록합니다. "Firebase Hosting도 설정" 체크는 하지 않아도 됩니다.
3. 등록하면 아래처럼 생긴 `firebaseConfig` 객체가 나옵니다. 이 값을 전부 복사하세요.
   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "your-project.firebaseapp.com",
     projectId: "your-project",
     storageBucket: "your-project.appspot.com",
     messagingSenderId: "...",
     appId: "..."
   };
   ```
4. `index.html` 파일을 열어 맨 위쪽 `var firebaseConfig = { ... };` 부분을 위에서 복사한 값으로 통째로 바꿔줍니다.
5. Firebase 콘솔 왼쪽 메뉴에서 **Build > Firestore Database**로 이동해 데이터베이스를 만듭니다 (위치는 가까운 리전으로, 예: `asia-northeast3`).
6. **규칙(Rules)** 탭으로 이동해서 아래 내용으로 전체 교체하고 **게시(Publish)**합니다.
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }
   ```
   > ⚠️ 이 규칙은 링크와 프로젝트 정보를 아는 사람은 누구나 데이터를 읽고 쓸 수 있게 열어두는 규칙이에요. 로그인 없이 "누구나 들어올 수 있게" 하기 위한 선택이니, 개인적인 커플 앱 용도로만 가볍게 쓰고 민감한 정보는 넣지 마세요.
7. 변경한 `index.html`을 커밋하고 `main` 브랜치에 푸시하면 GitHub Pages에 자동 반영됩니다.

## 사이트 주소

GitHub Pages가 켜져 있다면 아래 주소로 접속할 수 있어요.

```
https://<GitHub 사용자명>.github.io/<저장소 이름>/
```

## 로컬 미리보기

빌드 과정이 없는 순수 HTML/JS라 `index.html`을 브라우저로 바로 열거나, VS Code의 Live Server 확장 등으로 열어도 됩니다.

## 사용법

- 처음 접속하면 두 사람 이름을 입력해서 시작해요.
- 각자 자신의 기기에서 "지금 누구세요?"로 본인 역할을 선택해요 (기기별로 저장돼요).
- 오늘 할 일을 체크하면 "완료 대기" 상태가 되고, 상대가 역할을 전환해서 "도장 찍어주기"를 눌러야 실제로 도장이 쌓여요.
- 오른쪽 위 톱니바퀴 아이콘에서 이름, 할 일(할 일마다 받는 도장 개수 포함), 각자의 쿠폰(이름/필요 도장 개수)을 자유롭게 관리할 수 있어요.
