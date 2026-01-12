# 📱 모바일 AB 테스트 사이트

팀별로 모바일 스크린샷을 비교하고 **실시간으로 투표 결과를 공유**할 수 있는 웹 애플리케이션입니다.

## ✨ 주요 기능

- **🔥 실시간 동기화**: Firebase 기반으로 모든 사용자가 동일한 대시보드를 실시간으로 확인
- **🔒 팀별 보안 관리**: 비밀번호로 팀 데이터 보호
- **📱 모바일 최적화**: 모바일 스크린샷 비교에 최적화된 UI
- **💬 선택 이유 수집**: 투표자들의 피드백을 실시간으로 수집
- **🚫 중복 투표 방지**: 한 사용자당 한 번만 투표 가능
- **🔗 URL 공유**: 팀별 전용 URL로 쉽게 공유
- **📊 실시간 결과**: 투표 결과와 피드백을 실시간 시각화

## 🎯 사용 사례

- **디자인 부트캠프**: 학생들의 디자인 작업물 비교 평가
- **제품 팀**: 신규 UI/UX 디자인 A/B 테스트
- **프로토타입 검증**: 여러 디자인 시안 중 최적안 선정
- **사용자 조사**: 실제 사용자들의 선호도 조사

## 🚀 빠른 시작

### 1. Firebase 설정 (5분)
자세한 설정 방법은 [FIREBASE_SETUP.md](FIREBASE_SETUP.md) 참고

**요약:**
1. Firebase 콘솔에서 프로젝트 생성
2. Realtime Database 활성화
3. 웹 앱 등록 후 설정 정보 복사
4. `ab-test-firebase.html` 파일의 firebaseConfig에 붙여넣기

### 2. 배포하기

#### GitHub Pages로 배포
```bash
1. GitHub 저장소 생성
2. ab-test-firebase.html 업로드
3. Settings > Pages에서 배포 설정
4. https://your-username.github.io/repo-name/ab-test-firebase.html 에서 접속
```

#### Firebase Hosting으로 배포 (추천)
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

### 3. 사용하기
1. 팀 이름과 비밀번호 입력
2. 모바일 스크린샷 2개 업로드
3. 공유 링크 복사해서 팀원들에게 전달
4. 실시간으로 투표 결과 확인!

## 🔒 보안 기능

### 구현된 보안 기능
- ✅ **비밀번호 해시**: SHA-256으로 비밀번호 암호화 저장
- ✅ **XSS 방지**: 모든 사용자 입력 sanitize 처리
- ✅ **중복 투표 방지**: 사용자별 고유 ID로 관리
- ✅ **입력 검증**: 파일 크기/타입, 특수문자 필터링
- ✅ **Firebase 보안 규칙**: 서버 레벨 데이터 검증
- ✅ **Rate Limiting**: Firebase 자체 제한으로 과부하 방지

### 추가 보안 설정 (선택)
- 도메인 제한 (Firebase 콘솔에서 설정)
- API 키 제한 (Google Cloud Console)
- 더 강한 보안 규칙 적용

## 💻 기술 스택

- **Frontend**: 순수 HTML/CSS/JavaScript (프레임워크 없음)
- **Database**: Firebase Realtime Database
- **Authentication**: Password Hashing (SHA-256)
- **Hosting**: GitHub Pages / Firebase Hosting

## 📁 파일 구조

```
mobile-ab-test/
├── ab-test-firebase.html     # 메인 애플리케이션 (Firebase 연동)
├── FIREBASE_SETUP.md          # Firebase 설정 가이드
└── README.md                  # 이 파일
```

## 📊 데이터 구조

Firebase Realtime Database에 저장되는 데이터:

```javascript
{
  "teams": {
    "A팀": {
      "teamName": "A팀",
      "password": "hashed_password",  // SHA-256 해시
      "imageA": "data:image/png;base64,...",
      "imageB": "data:image/png;base64,...",
      "votesA": 10,
      "votesB": 8,
      "reasonsA": {
        "reason_1736745600000": {
          "text": "색상이 더 눈에 띄어요",
          "timestamp": "2026-01-12T..."
        }
      },
      "reasonsB": {...},
      "voters": {
        "user_1736745600000_abc123": {
          "option": "A",
          "timestamp": "2026-01-12T..."
        }
      },
      "createdAt": "2026-01-12T..."
    }
  }
}
```

## 🎓 교육용 활용 예시

### OZ 코딩스쿨 디자인 부트캠프
1. **팀 프로젝트 피드백**: 각 팀의 작업물을 실시간으로 비교
2. **디자인 리뷰**: 학생들이 서로의 작업에 피드백 제공
3. **포트폴리오 선정**: 여러 버전 중 최종 포트폴리오 결정
4. **사용성 테스트**: 실제 사용자 피드백 수집

## 💡 사용 팁

### 효과적인 테스트 운영
- **명확한 팀 이름**: 날짜나 목적 포함 (예: "2024-01-12-회원가입화면")
- **충분한 샘플**: 최소 10명 이상의 투표 권장
- **이유 작성 독려**: 구체적인 피드백을 위해 이유 작성 장려
- **테스트 목적 공유**: 무엇을 비교하는지 사전 안내

### 비밀번호 관리
- 팀원들과 사전에 비밀번호 공유
- 간단하지만 추측하기 어려운 비밀번호 사용
- 민감한 프로젝트는 강력한 비밀번호 사용

## 💰 비용

Firebase 무료 플랜(Spark)으로 충분히 사용 가능:
- **동시 접속**: 100명
- **저장 용량**: 1GB
- **데이터 전송**: 10GB/월

**학급/팀 단위 사용에 완전 무료입니다!**

## 🐛 문제 해결

### 자주 발생하는 문제

**Q: "Permission denied" 에러가 나요**
A: Firebase 보안 규칙을 확인하세요. [FIREBASE_SETUP.md](FIREBASE_SETUP.md) 참고

**Q: 데이터가 저장이 안 돼요**
A: Firebase 설정 정보(apiKey, databaseURL)가 정확한지 확인하세요.

**Q: 이미지가 업로드 안 돼요**
A: 이미지 크기가 2MB 이하인지, 이미지 파일 형식(JPG/PNG)인지 확인하세요.

**Q: 실시간 업데이트가 안 돼요**
A: 브라우저 개발자 도구(F12) 콘솔에서 에러 메시지를 확인하세요.

## 📱 브라우저 지원

- ✅ Chrome/Edge (권장)
- ✅ Safari
- ✅ Firefox
- ✅ 모바일 브라우저

## 🤝 기여하기

개선 제안이나 버그 리포트는 Issue로 등록해주세요!

## 📄 라이선스

Copyright (c) 2026 Hyemin (민트쌤)

이 소프트웨어의 모든 권리는 저작권자에게 있습니다.
개인적 또는 교육적 목적으로 자유롭게 사용 가능합니다.
상업적 사용이나 재배포 시에는 저작권자의 사전 허가가 필요합니다.

---

## 👩‍🏫 제작자

**Hyemin (민트쌤)**
- OZ Coding School 디자인 강사
- 디자인 교육 및 UX 리서치 전문

---

**궁금한 점이나 개선 제안이 있으시면 Issue를 남겨주세요!** 🎉
