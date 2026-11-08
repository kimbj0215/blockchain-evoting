# blockchain-evoting

블록체인 기반 전자투표 시스템 **프로토타입**과 그 위에서 진행하는 **블록체인 보안 실험** 저장소입니다.
실제 서비스가 아니라, "블록체인 전자투표가 있다면 이렇게 동작한다"를 보여주고 직접 공격·방어해 보는 것이 목표입니다.

## 구성

| 구분 | IP | 역할 |
| --- | --- | --- |
| Host PC | 192.168.56.1 | 유권자 단말기, 개발 환경 (MetaMask) |
| VM 1 | 192.168.56.10 | 선거관리위원회: Besu 노드 1, FastAPI, PostgreSQL |
| VM 2 | 192.168.56.20 | 시민단체/참관인: Besu 노드 2 |

- 블록체인: Hyperledger Besu (QBFT, 가스비 0) — 선정 이유는 [ADR-0001](docs/adr/0001-besu-instead-of-goquorum.md)
- 스마트 컨트랙트: Solidity, Hardhat
- 서버: Python FastAPI, PostgreSQL
- 파이프라인: GitHub Actions (gitleaks, 테스트, Slither)

## 진행 방식: 간소화 스크럼 + DevSecOps

- 스프린트: 1주 (GitHub Milestone으로 관리)
- 백로그: GitHub Issues + Projects 보드
- 모든 변경은 브랜치 → Pull Request → CI 통과 후 main 병합
- 매 스프린트 종료 시 회고 작성 (`docs/retro/`)

### 완료의 정의 (Definition of Done)

Issue를 Done으로 옮기려면 아래를 모두 만족해야 합니다.

1. Issue에 적은 완료 기준을 만족한다
2. Pull Request로 병합했고 CI가 모두 통과했다
3. 직접 실행해서 동작을 확인했다 (캡처 또는 로그를 Issue/PR에 첨부)
4. 관련 문서(README, docs)를 갱신했다

## 폴더 구조

```
.github/      Issue·PR 템플릿, CI 워크플로
infra/        Besu 설정, VM 네트워크 설정, 실행 스크립트
contracts/    Hardhat 프로젝트 (투표 스마트 컨트랙트)
backend/      FastAPI 유권자 인증 서버
web/          투표 웹 페이지
attacks/      보안 실험 공격 스크립트 (S-01 ~ S-10)
docs/         ADR, 회고, 보안 실험 보고서
```

## 보안 원칙

- 노드 개인키, MetaMask 시드 문구, 비밀번호는 절대 커밋하지 않는다 (`.gitignore`, GitHub Secrets 사용)
- 모든 공격 실험은 내 PC 안의 Host PC ↔ VM 사이에서만 수행한다
