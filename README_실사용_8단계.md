# 학생자치회 QR 보물찾기 v2.5.1 — 실사용 연결 안내

이 폴더의 `index.html`은 학생용/관리자용 웹앱입니다. **Firebase 프로젝트를 만드는 작업과 GitHub Pages 활성화는 학교 담당자가 본인 계정에서 수행해야 합니다.**
기존 v2.5와 동일한 80개 QR 코드 및 12종 보물 배치를 유지합니다. 보물 캐릭터 이미지는 추후 등록할 수 있습니다.

## 준비물
- Google 계정 (Firebase 프로젝트를 생성할 수 있어야 함)
- GitHub 계정
- 모바일 휴대전화 2대 (실제 동시 접속 테스트)

## 1단계. Firebase 프로젝트 및 웹 앱 등록
1. https://console.firebase.google.com/ 에서 '프로젝트 추가' → 예: `school-treasure-hunt` (실제 ID는 중복되지 않게).
2. 프로젝트에서 웹(</>) 아이콘 → 앱 닉네임(예: '학생자치회 보물찾기') → 앱 등록.
3. 'SDK 설정 및 구성'의 **구성(Config)**에서 `const firebaseConfig = {...};` 내용을 복사.
4. 배포 패키지의 `firebase-config.js`를 메모장/VS Code에서 열고 `window.TREASURE_FIREBASE_CONFIG = null;`을 `window.TREASURE_FIREBASE_CONFIG = { ... };`로 교체해 저장. `const firebaseConfig =` 대신 `window.TREASURE_FIREBASE_CONFIG =`를 사용.
5. `firebase-config.js`에는 관리자 계정 암호나 서비스 계정 키를 넣지 않습니다. 구성 객체 자체는 비밀번호가 아닙니다.

## 2단계. Firebase Authentication
1. Firebase 프로젝트 → Authentication → 시작하기.
2. 'Sign-in method/로그인 방법'에서 **익명(Anonymous)** 활성화 (학생의 별도 로그인 없음).
3. **이메일/비밀번호** 활성화 (관리자용).

## 3단계. Firestore Database 및 규칙
1. 프로젝트 → Firestore Database → 데이터베이스 만들기 → 기본 `(default)` DB 생성 (지역 선택은 나중에 변경 어려움).
2. 생성 후 **규칙(Rules)** 탭에서 이 폴더의 `firestore.rules` 내용을 전체 붙여넣고 **게시(Publish)**.
3. **테스트 모드의 모든 접근 허용 규칙을 유지하면 안 됩니다.** 반드시 제공한 rules를 게시합니다.

## 4단계. 관리자 계정 만들기
1. Authentication → 'Users/사용자' → 사용자 추가 → 관리자 이메일/비밀번호 설정.
2. 생성한 관리자 사용자의 **UID**를 복사합니다.
3. Firestore → 데이터(Data) → 컬렉션 시작 → 컬렉션 ID `admins`.
4. 문서 ID에 관리자 UID를 그대로 넣고, 필드 `role` (문자열) = `admin` 저장.
5. UID는 보통 긴 영문/숫자입니다. 이메일 주소를 문서 ID로 넣으면 작동하지 않습니다.

## 5단계. GitHub Pages로 HTTPS 배포
1. GitHub에서 새 public 저장소 생성 (예: `school-treasure-hunt`).
2. 저장소 첫 화면에서 Add file → Upload files → 이 폴더의 `index.html`, `firebase-config.js`, `.nojekyll` 업로드 → Commit.
   - `firestore.rules`·`storage.rules`는 **GitHub Pages 배포에 필요하지 않으므로** 공개 저장소에는 올리지 않아도 됩니다.
3. 저장소 → Settings → Pages → Build and deployment → Source 'Deploy from a branch' → Branch `main` / `(root)` → Save.
4. 표시된 `https://<username>.github.io/<repository>/` 주소를 확인합니다. (반영까지 시간이 걸릴 수 있음.)
5. 인터넷 접속 시 웹앱 상단에 `☁ 실시간` 또는 `서버 준비 필요` 상태가 나타나야 합니다. `🧪 체험`이면 설정 파일이 적용되지 않은 것입니다.

## 6단계. 서버 기본 콘텐츠 최초 등록
1. HTTPS 배포 주소 접속 → 관리자 메뉴 → 관리자 이메일/비밀번호로 로그인.
2. 관리자 → 설정 → **초기 콘텐츠 서버에 등록** 클릭.
3. 처음 등록 시 행사 상태는 **준비 중**으로 저장되므로 학생이 보물 기록을 쌓지 않습니다.
4. 관리자 → QR 관리에서 보물 12개 / 꽝 68개 확인.
5. 관리자 → 설정에서 학생 학년별 반 개수 등 수정.

## 7단계. 선택: Firebase Storage 이미지 업로드
- 실제 이미지 업로드 기능을 쓰려면 Firebase Storage를 활성화해야 하며, **2026년 현재 Firebase Storage는 Blaze(사용량 기반 결제) 요금제가 필요**합니다. 무료 사용 구간이 있더라도 과금 가능성이 있으니 결제 경보를 설정하세요.
- 이미지 제작 전에는 Storage 없이 이모지 카드와 기존 텍스트 꽝으로 테스트할 수 있습니다. 다만 이미지 업로드 버튼은 Storage 설정 전에는 작동하지 않습니다.
- 사용할 경우 Firebase → Storage → 시작하기 → 버킷 생성 → 규칙(Rules)에 `storage.rules` 게시.
- Storage와 Firestore는 같은 Firebase 프로젝트여야 합니다.

## 8단계. 스마트폰 실사용 점검
1. PC에서 관리자 로그인 상태로 행사 상태를 `진행 중`으로 잠시 전환합니다.
2. 학생폰 1에서 학년→반→번호→이름을 등록, 코드 직접 입력 `MY8V2C`로 보물 획득 확인.
3. 학생폰 2에서 다른 이름으로 같은 코드를 입력, 두 기기 모두 도감 1/12 확인, **최초 발견자는 한 명만 표시**되는지 확인.
4. `KG7F4P`를 2회 입력, 탐험 QR 수는 1개만 증가, 꽝 누적은 2회인지 확인.
5. 'QR 스캔'을 눌러 카메라 권한 허용 및 실제 종이 QR 인식 테스트 (아이폰/안드로이드 권장).
6. 관리자 상황판에 두 학생의 기록이 보이는지 확인.
7. 테스트 완료 후 관리자 → 설정 → **준비 중** 복귀, 관리자 → 설정 → 참여 기록 초기화.
8. **반드시 최종 HTTPS 배포 주소에서** QR 80개를 인쇄 또는 PDF 저장합니다.
9. 행사 시작 시 관리자 → 설정 → `진행 중`으로 변경합니다.

## 문제 해결
- `🧪 체험`: firebase-config.js가 서버에 업로드되지 않았거나, 해당 파일 내용이 여전히 null.
- `서버 준비 필요`: 관리자 로그인 후 '초기 콘텐츠 서버에 등록'을 먼저 실행.
- `auth/operation-not-allowed`: Authentication → 익명 활성화 확인.
- `permission-denied`: firestore.rules 게시 여부 및 Firestore admins/{관리자UID} 문서 확인.
- `storage/unauthorized` 또는 402: Storage rules/Blaze 요금제 확인.
- 기본 카메라 불가: 웹앱 내 QR 스캐너 또는 6자리 수동 입력으로 진행.
- 기존 QR 인쇄물이 있다면 **배포 주소 변경 시 인쇄된 QR이 더는 올바르지 않을 수 있음**. 반드시 URL 확정 후 출력.

## 데이터와 개인정보
- 학생이 학년·반·번호·이름을 입력하고 본인 스마트폰에서 이용하는 간편 참여 방식입니다. 학생 본인 여부를 엄격히 검증하지 않습니다.
- 학생 명단/CSV는 공개 GitHub 저장소에 절대 업로드하지 마세요. Firestore 규칙에서 학생별 기록은 본인 및 관리자만 읽도록 구성돼 있습니다.
- 관리자 비밀번호는 파일/채팅/공개 저장소에 넣지 마세요.
- 익명 인증 상태는 브라우저 저장정보를 삭제하거나 기기를 바꾸면 이어서 복원되지 않을 수 있습니다.
- 행사 후 기록은 학교 내부 개인정보 처리 지침에 따라 적절한 시점에 삭제하세요.
