# Google Apps Script에서 Supabase로의 마이그레이션 가이드

이 문서는 '우리집 DayCareCenter' 프로젝트를 Google Apps Script에서 Supabase로 마이그레이션하는 과정을 설명합니다.

## 마이그레이션 개요

### 기존 시스템

- Google Apps Script와 Google 스프레드시트 기반
- `code.gs` 파일로 서버 로직 구현
- `index.html`에서 `google.script.run`을 통해 서버 함수 호출

### 새 시스템

- Supabase 데이터베이스 기반
- `supabase.js` 파일로 서버 로직 구현
- 클라이언트-서버 통신이 비동기 Promise 기반으로 변경

## 주요 변경 사항

1. **데이터 스토리지**: Google 스프레드시트에서 Supabase PostgreSQL 데이터베이스로 변경
2. **ID 생성 방식**: 직접 생성하던 방식에서 Supabase의 UUID 자동 생성 기능으로 변경
3. **날짜/시간 저장 방식**: 텍스트 형식에서 숫자(int4) 형식으로 변경
4. **비동기 처리**: 동기식 호출에서 Promise 기반 비동기 처리로 변경

## 마이그레이션된 파일

1. **index.html**:

   - Supabase 라이브러리 스크립트 추가
   - `google.script.run` 호출을 `async/await` 기반 함수로 변경
   - 기타 처리 로직 수정

2. **supabase.js** (신규):

   - Supabase 클라이언트 초기화
   - `code.gs`의 기능을 Supabase API 호출로 구현

3. **README.md** (신규):
   - 프로젝트 개요 및 사용 방법 설명

## 데이터 구조 변경

### 테이블 구조

- `activities_plan`: 일일 활동 계획 및 기록
- `employeesinfo`: 직원 정보

### 데이터 타입 변경

- 날짜: `YYYY-MM-DD` 문자열 → `YYYYMMDD` 형식의 숫자(int4)
- 시간: `'HHMM'` 문자열 → `HHMM` 형식의 숫자(int4)
- ID: 직접 생성한 문자열 → 자동 생성되는 UUID

## 추가 작업

1. **데이터 마이그레이션**:

   - 기존 Google 스프레드시트의 데이터를 Supabase 테이블로 가져오기
   - 날짜 및 시간 형식 변환 작업 필요

2. **테스트**:

   - 모든 기능이 예상대로 작동하는지 테스트
   - 특히 날짜/시간 형식 변환 로직 확인

3. **보안 설정**:
   - Supabase의 RLS(Row Level Security) 설정 검토 및 적용

## 주의 사항

1. **API 키 보안**:

   - Supabase API 키가 노출되지 않도록 주의
   - 프로덕션 환경에서는 환경 변수 등으로 관리 권장

2. **브라우저 호환성**:
   - Supabase JS 클라이언트는 최신 브라우저 지원
   - IE 등 구형 브라우저 지원이 필요한 경우 폴리필 추가 필요

## 문제 해결

마이그레이션 과정에서 발생할 수 있는 일반적인 문제:

1. **날짜/시간 형식 오류**:

   - 날짜와 시간이 올바르게 변환되지 않을 경우 데이터 변환 로직 확인

2. **CORS 오류**:

   - Supabase 프로젝트 설정에서 적절한 오리진 설정 필요

3. **비동기 처리 오류**:
   - Promise 체인과 에러 처리가 올바르게 구현되었는지 확인
