# YeongduMusic Web

Cloudflare Pages에 배포하는 YeongduMusic 공식 안내 사이트입니다.

- 홈페이지: https://music.yeongdu.xyz/
- 개인정보처리방침: https://music.yeongdu.xyz/privacy/
- 서비스 이용약관: https://music.yeongdu.xyz/terms/

## 구성

```text
public/
  index.html              # 메인 소개
  privacy/index.html      # Google OAuth 개인정보처리방침(확인 필수)
  terms/index.html        # 이용약관
  404.html
  assets/styles.css       # 반응형 공통 스타일
  assets/site.js          # 모바일 메뉴 및 저작권 연도
  assets/favicon.svg      # 자체 제작 음파 모티프 파비콘
  _headers                # 응답 보안 헤더
  robots.txt
  sitemap.xml
```

프레임워크, npm, 빌드 도구, 외부 폰트/트래킹 스크립트 없이 작동하는 정적 웹사이트입니다.

## ⚠️ 반드시 수정한 후 Google OAuth 심사 신청

이 사이트는 **데이터 처리 방식이 아직 확인되지 않은 초안**입니다. 현재 개인정보처리방침이 Google OAuth 심사를 통과할 것이라고 보장하지 않습니다.

1. `CONTACT_EMAIL_HERE`를 실제로 메일을 받을 수 있는 이메일 주소로 모두 교체하세요. (`public/index.html`, `public/privacy/index.html`, `public/terms/index.html`)
2. `public/privacy/index.html`의 **4. 저장 및 보관 기간**에 인증 토큰 저장소(예: Expo SecureStore), 캐시, 다운로드 파일 위치, 삭제 방식을 실제 코드에 맞춰 명시하세요.
3. **5. 외부 서비스 이용 및 정보 제공**에 Google Drive 요청이 `앱 → Google API` 직접인지, 본인 서버(NAS 포함)를 거치는지 명시하세요. 분석 SDK/크래시 리포팅/서버 로그 등도 검토하세요.
4. Google OAuth 스코프를 확인하세요. `drive.readonly`, `drive.metadata.readonly` 등은 Google 정책상 restricted scope입니다. 실제 권한을 최소화하고 공개 배포에 필요한 심사 조건을 확인하세요.
5. **6. Google API 사용자 데이터 사용**에 적힌 이용 제한을 실제 구현이 준수하는지 확인하세요.
6. 실제 미지원 기능이 있다면 홈페이지 설명을 수정하세요. 음악 플레이어 이미지의 트랙은 가상의 **예시 UI**이며 앱 스크린샷이 아닙니다.
7. 모든 개인정보처리방침의 노란색 '공개 전 확인 항목' 박스를 검증 후 삭제하거나 확정된 처리 방식의 문구로 교체하세요.

### 로컬 확인

별도의 패키지 설치는 필요 없습니다. 파일 경로가 정상 동작하는지 로컬 정적 서버를 켜고 확인하세요.

```bash
cd public
python -m http.server 8000
```

- `http://localhost:8000/`
- `http://localhost:8000/privacy/`
- `http://localhost:8000/terms/`

### GitHub → Cloudflare Pages 배포

1. GitHub에서 `YeongduMusic-Web` 저장소를 생성하고 이 폴더의 파일을 업로드/푸시합니다.
2. Cloudflare Dashboard → Workers & Pages → Create → Pages → **Connect to Git**에서 저장소를 연결합니다.
3. 아래처럼 설정하고 배포합니다.

| 항목 | 값 |
|---|---|
| Project name | `yeongdumusic` |
| Framework preset | `None` |
| Production branch | `main` |
| Root directory | `/` 또는 비워두기 |
| Build command | 비워두기 |
| Build output directory | `public` |

4. 첫 배포 확인 후 Pages 프로젝트의 **Custom domains → Set up a custom domain**에서 `music.yeongdu.xyz`를 연결합니다. Cloudflare DNS에 동일 이름의 기존 Tunnel CNAME 등 충돌 레코드가 없는지 확인하세요.
5. 아래 세 URL이 로그인 없이 HTTPS로 열리는지 확인합니다.
   - https://music.yeongdu.xyz/
   - https://music.yeongdu.xyz/privacy/
   - https://music.yeongdu.xyz/terms/

### Google OAuth 브랜딩

1. Google Search Console에서 `yeongdu.xyz` 도메인 속성을 추가하고 DNS TXT 레코드로 소유권을 확인합니다. Google OAuth 프로젝트에 등록된 Google 계정에 적절한 권한이 있어야 합니다.
2. Google Auth Platform → **Branding**에서 다음 주소를 등록합니다.

| 항목 | URL 또는 값 |
|---|---|
| 앱 홈페이지 | `https://music.yeongdu.xyz/` |
| 개인정보처리방침 | `https://music.yeongdu.xyz/privacy/` |
| 서비스 이용약관 | `https://music.yeongdu.xyz/terms/` |
| 승인된 도메인 | `yeongdu.xyz` |

3. Google Drive에 필요한 스코프가 민감/제한 범위인지 확인하고, 앱 게시/검증 절차를 별도로 진행합니다. **홈페이지와 개인정보처리방침이 있다고 해서 Google Drive 접근이 자동 승인되는 것은 아닙니다.**

Google 공식 참고 문서:
- https://developers.google.com/identity/protocols/oauth2/production-readiness/policy-compliance
- https://developers.google.com/identity/protocols/oauth2/production-readiness/brand-verification
- https://developers.google.com/workspace/drive/api/guides/api-specific-auth
- https://developers.cloudflare.com/pages/get-started/git-integration/

## 배포 전 빠른 점검

```bash
# 실제 연락처로 전부 교체했는지 확인
rg 'CONTACT_EMAIL_HERE' public
# 개인정보처리방침 검토용 경고 문구가 남아 있는지 확인
rg '공개 전 확인|배포 전 점검' public/privacy/
```
