# Studio HaiP — Homepage

## 인프라 현황
| 항목 | 내용 |
|------|------|
| 도메인 | haip.kr (Gabia) |
| 서버 | AWS EC2 — Amazon Linux 2 |
| 퍼블릭 IP | 13.221.115.180 |
| 웹서버 | Nginx |
| GitHub | https://github.com/angkmfirefoxygal/haip-hompage |

## EC2 접속
```bash
ssh -i ~/Downloads/haip-home.pem ec2-user@13.221.115.180
```

---

## 개발

### 로컬 실행
```bash
python3 -m http.server 3000
# http://localhost:3000
```

### 코드 수정 → 배포

**1. 로컬에서**
```bash
git add .
git commit -m "커밋 메시지"
git push
```

**2. EC2에서**
```bash
ssh -i ~/Downloads/haip-home.pem ec2-user@13.221.115.180
bash /var/www/haip/deploy.sh
```
`deploy.sh`가 git pull + nginx reload 자동 처리.

---

### Nginx 설정 변경 시 (추가 작업 필요)
```bash
# EC2에서
sudo cp /var/www/haip/nginx/haip.conf /etc/nginx/conf.d/haip.conf
sudo nginx -t && sudo systemctl reload nginx
```

> **주의**
> - Nginx 설정 경로: `conf.d` (sites-available 아님 — Amazon Linux 2)
> - cp 전 반드시 `nginx -t`로 문법 확인
> - HTTPS 인증서는 certbot 관리 중 → nginx.conf 덮어쓰면 443 끊김
> - 복구 필요 시: `sudo certbot --nginx -d haip.kr -d www.haip.kr` → 옵션 `1` 선택

---

## 파일 구조
```
haip_hompage/
├── index.html          # 홈 (Hero / Stats / Services / Works / Footer)
├── services.html       # 서비스 상세
├── portfolio.html      # 포트폴리오 + 카테고리 필터
├── contact.html        # 문의 폼
├── about.html          # 회사 소개
├── css/style.css       # 디자인 시스템 (CSS Variables)
├── js/main.js          # Nav, 스크롤 리빌, 숫자 카운터
├── assets/images/
│   ├── Ellipse 680.png       # 서비스 섹션 원형 배경
│   ├── image 8407.png        # Hince 브랜드 홈페이지 썸네일
│   ├── Group 6.png           # Growth Campaign 썸네일
│   └── mobile-app-uxui.png   # Mobile APP UX/UI 썸네일
└── nginx/haip.conf     # Nginx 서버 블록
```

---

## 브랜드
| 역할 | 컬러 |
|------|------|
| 로고 / CTA / 포인트 | `#0425FF` Electric Blue |
| 서비스 원 / 푸터 배경 | `#F3FF94` Lime Yellow |
| 배경 | `#FFFFFF` White |

**폰트**: Pretendard Variable (CDN)

## 연락처
| | |
|--|--|
| Email | haip.office@gmail.com |
| Phone | 010-9913-8511 |
| Instagram | @studio.haip |

---

## HTTPS
Let's Encrypt 인증서 적용 완료. 자동 갱신 활성화됨.
- https://haip.kr
- https://www.haip.kr
