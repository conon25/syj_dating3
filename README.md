syj_dating3 - Flutter app and spring-cloud micro services
========================================================
# 아키텍처
<img
  src="./소프트웨어 구성도2.png"
  width="900"
  height="450"
/>

# 주요기능
### 로그인
<img
  src="./images/intro_login.jpg"
  width="218"
  height="468"
/>

#### 이메일 로그인
* 회원가입시 등록한 이메일과 비밀번호로 로그인합니다. 로그인에 성공하면 JWT토큰을 부여받아 secure_storage에 저장되어 다음에 앱을 켤때 자동으로 로그인 합니다.
#### 구글 로그인
* 구글 계정으로 로그인합니다. 로그인 성공시 Firebase_auth를 통해 ID토큰을 발급받고 이 토큰을 이용해서 로그인합니다. 서버에서 ID토큰이 유효한지 검증하고 새로운 JWT토큰을 부여받습니다.
#### 로그인 이력 저장
* 로그인 시 IP, 기기모델 등 데이터를 서버에 저장합니다.

<br/>

### 회원 가입

<img
  src="./images/agree_service.jpg"
  width="218"
  height="468"
/>

#### 이메일로 프로필 생성하기
* 이메일과 비밀번호를 입력하여 계정을 생성합니다. 비밀번호는 SHA512로 암호화 하여 저장됩니다.
#### 구글계정으로 프로필 생성하기
* 구글 계정으로 로그인하여 회원가입하고 프로필 정보만 작성합니다. 마지막 회원가입 요청시 유효한 ID토큰인지 검증하고 회원가입을 최종처리합니다.

<br/>

### 전화번호 인증

<img
  src="./images/phone_check.jpg"
  width="218"
  height="468"
/>
<img
  src="./images/phone_check2.jpg"
  width="218"
  height="468"
/>

#### 인증코드 받기
* 전화번호를 입력하고 전화번호로 인증번호를 받습니다.
#### 인증코드 검증하기
* 유효한 인증번호일 경우 ID토큰을 부여받습니다. 최종 회원가입 요청시 Id토큰을 검증합니다.

<br/>

### 회원가입 사진등록
<img
  src="./images/signup_reg_photo.jpg"
  width="218"
  height="468"
/>
#### 사진 업로드하기
* 카메라및 갤러리에서 사진을 선택하고 업로드합니다.
* 최대 6장까지 업로드 가능합니다.

<br/>

### 내 프로필 설정 및 정보
<img
  src="./images/my_profile.jpg"
  width="218"
  height="468"
/>
#### 프로필 보기
* 내 프로필을 조회하고 수정합니다.
#### 앱 설정
* 설정 앱으로 이동합니다.
#### 로그아웃
* secure_storage 및 계정 데이터를 모두 지우고 로그인 페이지로 이동합니다.

<br/>

### 프로필 상세
<img
  src="./images/profile.jpg"
  width="218"
  height="468"
/>
#### 프로필 수정
* 내 프로필을 조회하고 수정합니다.

<br/>

### 내 근처 친구찾기 리스트
<img
  src="./images/around_list_page.jpg"
  width="218"
  height="468"
/>
#### 위치 권한 허용 및 내 위치 lat long 가져오기
* 위치 권한을 허용시 앱이 resume될 때 내 기기 위치의 lat long을 측정하고 서버에 저장합니다.
* lat long 값을 기반으로 근처 사용자의 리스트를 보여줍니다.
#### reverse geocoding으로 지역명 표시
* 구글 맵 API를 사용하여 지역명을 표시합니다.
<br/>

### 프로필 선택 및 좋아요 보내기
<img
  src="./images/profile_detail_sendlike.jpg"
  width="218"
  height="468"
/>
#### 좋아요 보내기
* 받은 좋아요 목록에 없는상대, 현재 채팅중이 아닌 상대, 보낸 좋아요 목록에 없는 상대 조건을 충족한 상대에게 Like를 보낼 수 있습니다.
* socket연결을 통해 실시간으로 좋아요 메세지를 보내며 알림 권한을 허용한 기기에는 FCM을 통해 Notification을 표시합니다.
* 네트워크 오류로 연결이 끊기면 연결을 재시도합니다
<br/>

### 받은 좋아요 리스트
<img
  src="./images/received_like2.jpg"
  width="218"
  height="468"
/>
#### 좋아요 메시지 수신
* 포그라운드 상태일때 socket연결을 통해 실시간으로 좋아요 메세지를 받습니다. 백그라운드 상태일때는 socket연결이 끊긴 상태이므로 앱이 resume 되면 DB에서 내역을 불러옵니다.
* FCM을 통해 앱이 백그라운드 및 절전모드에 있어도 좋아요 Notification이 표시됩니다.
<br/>

### 좋아요 받기
<img
  src="./images/receivelike.jpg"
  width="218"
  height="468"
/>
#### 좋아요 받기
* 좋아요를 수락하면 현재 대화중이 아닌 상대일 경우 채팅방을 생성합니다.
<br/>

### 채팅방 리스트와 채팅방 개설
<img
  src="./images/chatroom_list.jpg"
  width="218"
  height="468"
/>
#### 채팅방 생성
* 채팅방 생성시 상대에게 socket 및 fcm을 통해 알림 메세지를 보냅니다.
<br/>

### 친구와 채팅
<img
  src="./images/chat_page.jpg"
  width="218"
  height="468"
/>
#### 채팅 데이터 송수신 및 이력 저장
* 채팅방 입장시 socket연결을 수립하고 실시간으로 데이터를 송수신 합니다.
* 상대가 앱이 백그라운드 상태이거나 종료 상태일 경우 FCM만 전송되고 채팅 내역은 DB에 저장됩니다.
* 채팅방 입장시 채팅내역을 불러온 후 secure_storage에 저장하고 이후 새로운 채팅 내역을 DB에서 불러올때 secure_storage 저장되지 않은 채팅내역만 불러오게하여 응답속도를 높였습니다.
<br/>

### 채팅방 나가기
<img
  src="./images/chat_page_exit.jpg"
  width="218"
  height="468"
/>
#### 채팅방 나가기 메세지 전송
* 채팅방에서 나가면 socket을 통해 퇴장 메시지를 전송하고 DB에 이력을 저장합니다.
#### 상대방에게 보여지는 프로필 정보 삭제
* 상대방에게 내 프로필을 더이상 표시하지 않습니다.
#### 채팅방 재생성
* 한 번 채팅이 종료된 상대라도 다시 좋아요를 보낼 수 있습니다. 재 생성시 이전 채팅 내역이 복구됩니다.
#### 대화내용 신고
* 상대방이 나가서 프로필이 표시되지 않아도 대화내용을 신고할 수 있습니다.
<br/>

### 매력평가 탭
<img
  src="./images/attraction_rating_page2.jpg"
  width="218"
  height="468"
/>
#### 매력평가할 프로필 받기
* 매시 정각에 내가 평가할 프로필이 새롭게 지정됩니다.
#### 매력평가하기
* rating bar 를 터치하여 매력을 평가합니다. socket연결을 통해 데이터를 전송하고 이력을 저장합니다.
* 상대에게 받은 평가는 1주일간 지속됩니다.
#### 매력평가 확인
* 내가 받은 매력평가는 받은별점 탭에서 확인할 수 있습니다.
* 별점 4점 이상을 준 경우 상세 프로필에 표시됩니다.
#### 매력평가 평점 확인
* 내가 받은 매력평가의 평균 값을 내 프로필 탭에서 확인할 수 있습니다.
* 내 매력평가 평점에 따라 추천 프로필 및 매력평가할 프로필 리스트가 달라집니다.

<br/>

### 친구 프로필 상세 페이지에서 매력 평가하기
<img
  src="./images/attraction_rating_detail_profile_page.jpg"
  width="218"
  height="468"
/>
#### 매력평가하기
* rating bar 를 터치하여 매력을 평가합니다. socket연결을 통해 데이터를 전송하고 이력을 저장합니다.
<br/>

### 매력평가할 프로필 대기상태
<img
  src="./images/attraction_rating_page_waiting2.jpg"
  width="218"
  height="468"
/>
#### 프로필을 모두 평가했을때
* 다음 정시가 될 때까지 남은시간을 표시합니다.
<br/>

### 불량유저 신고하기
<img
  src="./images/report_profile.jpg"
  width="218"
  height="468"
/>
#### 신고 접수
* 신고가 접수 되고 해당 사용자에 서비스를 제한할 수 있습니다.

<br/>
