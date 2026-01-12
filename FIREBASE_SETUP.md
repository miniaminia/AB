# 🔥 Firebase 설정 가이드

## 1. Firebase 프로젝트 만들기

### 단계 1: Firebase 콘솔 접속
1. https://console.firebase.google.com/ 접속
2. Google 계정으로 로그인
3. "프로젝트 추가" 클릭

### 단계 2: 프로젝트 생성
1. **프로젝트 이름**: `mobile-ab-test` (원하는 이름)
2. **Google 애널리틱스**: 선택 안 함 (간단하게 진행)
3. "프로젝트 만들기" 클릭
4. 1-2분 기다리면 완료

---

## 2. Realtime Database 설정

### 단계 1: 데이터베이스 만들기
1. 왼쪽 메뉴에서 **"빌드" > "Realtime Database"** 클릭
2. **"데이터베이스 만들기"** 버튼 클릭
3. **위치 선택**: `asia-southeast1` (싱가포르 - 한국과 가장 가까움)
4. **보안 규칙**: "테스트 모드에서 시작" 선택
5. **"사용 설정"** 클릭

### 단계 2: 보안 규칙 설정 (중요!)
1. 데이터베이스 만들어지면 **"규칙"** 탭 클릭
2. 다음 규칙으로 변경:

```json
{
  "rules": {
    "teams": {
      "$teamId": {
        ".read": true,
        ".write": true,
        ".validate": "newData.hasChildren(['teamName', 'password'])",
        "imageA": {
          ".validate": "newData.val().length < 3000000"
        },
        "imageB": {
          ".validate": "newData.val().length < 3000000"
        },
        "votesA": {
          ".validate": "newData.isNumber()"
        },
        "votesB": {
          ".validate": "newData.isNumber()"
        }
      }
    }
  }
}
```

3. **"게시"** 버튼 클릭

**이 규칙의 의미:**
- ✅ 모든 사람이 데이터를 읽고 쓸 수 있음
- ✅ 이미지 크기 제한 (3MB)
- ✅ 투표 수는 숫자만 가능
- ✅ 팀 이름과 비밀번호 필수

---

## 3. 웹 앱 등록

### 단계 1: 앱 추가
1. Firebase 콘솔 홈에서 **"웹" 아이콘 (</>)** 클릭
2. **앱 닉네임**: `AB Test Web` (원하는 이름)
3. **"Firebase 호스팅 설정"**: 체크 안 함
4. **"앱 등록"** 클릭

### 단계 2: 설정 정보 복사
다음과 같은 설정 정보가 나타납니다:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:xxxxxxxxxxxxx"
};
```

**이 정보를 복사해두세요!**

---

## 4. HTML 파일에 설정 적용

### ab-test-firebase.html 파일 열기

파일에서 다음 부분을 찾으세요 (71번째 줄 근처):

```javascript
// ⚠️ Firebase 설정 - 여기에 본인의 Firebase 설정을 넣으세요
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    databaseURL: "YOUR_DATABASE_URL",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

**위의 YOUR_XXX 부분을 복사한 실제 값으로 바꿔주세요!**

예시:
```javascript
const firebaseConfig = {
    apiKey: "AIzaSyABCDEFGHIJKLMNOPQRSTUVWXYZ",
    authDomain: "mobile-ab-test.firebaseapp.com",
    databaseURL: "https://mobile-ab-test-default-rtdb.asia-southeast1.firebasedatabase.app",
    projectId: "mobile-ab-test",
    storageBucket: "mobile-ab-test.appspot.com",
    messagingSenderId: "123456789012",
    appId: "1:123456789012:web:abc123def456"
};
```

---

## 5. 테스트하기

### 로컬에서 테스트
1. `ab-test-firebase.html` 파일을 브라우저로 열기
2. 팀 이름: `테스트팀`
3. 비밀번호: `1234`
4. "시작하기" 클릭

### 작동 확인
1. Firebase 콘솔 > Realtime Database > "데이터" 탭
2. `teams/테스트팀` 에 데이터가 생성되는지 확인

---

## 6. GitHub Pages 배포

### 방법 1: 직접 업로드
1. GitHub 저장소 생성
2. `ab-test-firebase.html` 파일 업로드
3. Settings > Pages에서 배포

### 방법 2: Firebase Hosting (추천!)
Firebase 자체 호스팅을 사용하면 더 빠르고 안정적입니다.

```bash
# Firebase CLI 설치
npm install -g firebase-tools

# 로그인
firebase login

# 초기화
firebase init hosting

# 배포
firebase deploy
```

배포 후 URL: `https://your-project.web.app`

---

## 🔒 보안 강화 (선택사항)

### 1. 도메인 제한
Firebase 콘솔 > Authentication > Settings > 승인된 도메인에 본인 도메인만 추가

### 2. API 키 제한
Google Cloud Console > API 및 서비스 > 사용자 인증 정보에서 API 키 제한 설정

### 3. 더 강한 보안 규칙
```json
{
  "rules": {
    "teams": {
      "$teamId": {
        ".read": true,
        ".write": "auth != null || (!data.exists() && newData.hasChildren(['password']))",
        "password": {
          ".validate": "newData.isString() && newData.val().length === 64"
        },
        "voters": {
          "$userId": {
            ".write": "!data.exists()"
          }
        }
      }
    }
  }
}
```

---

## 💰 비용 안내

### 무료 Spark 플랜 한도 (충분함!)
- **동시 접속**: 100명
- **저장 용량**: 1GB
- **데이터 전송**: 10GB/월
- **읽기/쓰기**: 무제한

**학생 20-30명 정도 사용하기에 충분합니다!**

### 비용 발생 시
만약 한도를 초과하면:
- Firebase가 자동으로 차단 (추가 비용 없음)
- Blaze 플랜으로 업그레이드 필요 (사용한 만큼만 과금)

---

## 🐛 문제 해결

### "Permission denied" 에러
→ Realtime Database 보안 규칙 확인

### 데이터가 저장 안 됨
→ Firebase 설정 (apiKey, databaseURL 등) 확인

### 이미지 업로드 안 됨
→ 이미지 크기 2MB 이하인지 확인

### 실시간 업데이트 안 됨
→ 브라우저 개발자 도구 콘솔에서 에러 확인

---

## 📞 도움 필요시

1. Firebase 공식 문서: https://firebase.google.com/docs/database
2. Firebase 콘솔에서 "지원" 메뉴 확인
3. Stack Overflow에서 "firebase realtime database" 검색

---

**설정 완료하면 바로 사용 가능합니다!** 🎉
