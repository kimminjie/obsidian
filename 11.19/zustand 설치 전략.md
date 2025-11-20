# Zustand Store 전략 문서

  

## 📋 목차

1. [프로젝트 구조 분석](#1-프로젝트-구조-분석)

2. [설치 및 설정](#2-설치-및-설정)

3. [Store 설계](#3-store-설계)

4. [구현 전략](#4-구현-전략)

5. [마이그레이션 계획](#5-마이그레이션-계획)

6. [확장성 고려사항](#6-확장성-고려사항)

7. [파일 생성 계획](#7-파일-생성-계획)

  

---

  

## 1. 프로젝트 구조 분석

  

### 현재 상황

- **프레임워크**: Next.js 16 (App Router)

- **언어**: TypeScript

- **상태 관리**: 현재 `page.tsx`에서 `useState`로 로컬 상태 관리

- **관리되는 상태**:

  - `query`: 검색어 입력값

  - `results`: 검색 결과 데이터

  

### 현재 코드 위치

- `frontend/app/page.tsx`: 검색 UI 및 상태 관리 로직

  

---

  

## 2. 설치 및 설정

  

### 2.1 의존성 설치

```bash

pnpm add zustand

```

  

### 2.2 디렉토리 구조

```

frontend/

├── app/

│   ├── store/              # 새로 생성할 디렉토리

│   │   ├── index.ts        # Store 인스턴스 (단일 store)

│   │   └── types.ts        # TypeScript 타입 정의

│   ├── layout.tsx

│   ├── page.tsx

│   └── ...

```

  

---

  

## 3. Store 설계

  

### 3.1 Store 타입 정의 (`app/store/types.ts`)

  

**필요한 타입들:**

- 검색 관련 상태 타입

- API 응답 데이터 타입

- Store 액션 타입

- 에러 상태 타입

  

**예상 구조:**

```typescript

// 검색 결과 타입

interface SearchResult {

  message?: string;

  data?: any[] | object;

}

  

// Store 상태 타입

interface StoreState {

  // 상태

  query: string;

  results: SearchResult | null;

  loading: boolean;

  error: string | null;

  // 액션

  setQuery: (query: string) => void;

  setResults: (results: SearchResult | null) => void;

  setLoading: (loading: boolean) => void;

  setError: (error: string | null) => void;

  reset: () => void;

  search: (keyword: string) => Promise<void>; // 선택사항

}

```

  

### 3.2 단일 Store 생성 (`app/store/index.ts`)

  

**핵심 원칙:**

- ✅ **반드시 1개의 store만 생성**

- ✅ Zustand의 `create` 함수 사용

- ✅ TypeScript로 타입 안정성 보장

  

**Store 구조:**

```typescript

import { create } from 'zustand';

import type { StoreState } from './types';

  

export const useStore = create<StoreState>((set) => ({

  // 초기 상태

  query: '',

  results: null,

  loading: false,

  error: null,

  // 액션들

  setQuery: (query) => set({ query }),

  setResults: (results) => set({ results }),

  setLoading: (loading) => set({ loading }),

  setError: (error) => set({ error }),

  reset: () => set({

    query: '',

    results: null,

    loading: false,

    error: null

  }),

}));

```

  

---

  

## 4. 구현 전략

  

### 4.1 Store 구조 패턴

  

**단일 Store 패턴:**

- 하나의 `useStore` 훅만 export

- 모든 상태와 액션을 하나의 store에 통합

- 필요시 선택적 구독으로 성능 최적화

  

**사용 예시:**

```typescript

// 전체 store 구독

const { query, setQuery } = useStore();

  

// 선택적 구독 (성능 최적화)

const query = useStore((state) => state.query);

const setQuery = useStore((state) => state.setQuery);

```

  

### 4.2 컴포넌트 통합 전략

  

**`page.tsx` 마이그레이션:**

1. `useState` 제거

2. `useStore` 훅 import

3. 상태와 액션을 store에서 가져오기

4. 기존 로직은 최대한 유지

  

**변경 전:**

```typescript

const [query, setQuery] = useState("");

const [results, setResults] = useState<any>(null);

```

  

**변경 후:**

```typescript

const query = useStore((state) => state.query);

const setQuery = useStore((state) => state.setQuery);

const results = useStore((state) => state.results);

const setResults = useStore((state) => state.setResults);

```

  

### 4.3 Context Provider (선택사항)

  

**Zustand는 Provider가 필수가 아님:**

- 기본적으로 Provider 없이 사용 가능

- SSR/하이드레이션 이슈가 있는 경우에만 Provider 사용

  

**Provider가 필요한 경우:**

```

app/

  store/

    StoreProvider.tsx  # Context Provider (선택사항)

    index.ts           # Store 인스턴스

```

  

**Provider 사용 예시:**

```typescript

'use client';

import { createContext, useContext } from 'react';

import { useStore } from './index';

  

const StoreContext = createContext(useStore);

  

export const StoreProvider = ({ children }) => {

  return (

    <StoreContext.Provider value={useStore}>

      {children}

    </StoreContext.Provider>

  );

};

```

  

---

  

## 5. 마이그레이션 계획

  

### 5.1 단계별 접근

  

#### Step 1: 타입 정의 작성

- `app/store/types.ts` 파일 생성

- 모든 필요한 타입 정의

  

#### Step 2: Store 생성

- `app/store/index.ts` 파일 생성

- 기본 상태와 액션 구현

  

#### Step 3: 컴포넌트 전환

- `app/page.tsx`에서 `useState` → `useStore` 전환

- 기존 로직 유지하면서 상태 관리만 변경

  

#### Step 4: 검색 로직 통합 (선택사항)

- `handleLog` 함수를 store의 `search` 액션으로 이동

- 비즈니스 로직을 store에 집중

  

#### Step 5: 테스트 및 검증

- 기능 동작 확인

- 타입 에러 확인

- 성능 최적화 (선택적 구독)

  

### 5.2 주의사항

  

**Next.js SSR 고려:**

- ✅ Store는 클라이언트 컴포넌트에서만 사용 (`"use client"`)

- ✅ 서버 컴포넌트에서는 사용 불가

  

**하이드레이션 이슈:**

- 서버와 클라이언트의 초기 상태가 다를 수 있음

- 필요시 `useEffect`로 클라이언트에서만 초기화

  

**타입 안정성:**

- TypeScript 타입 정의 필수

- `any` 타입 최소화

  

---

  

## 6. 확장성 고려사항

  

### 6.1 단일 Store 유지 원칙

- ✅ **반드시 1개의 store만 유지**

- ✅ 모든 상태를 하나의 store에 통합

- ✅ 필요시 슬라이스 패턴으로 모듈화 가능

  

### 6.2 슬라이스 패턴 (선택사항)

단일 store를 유지하면서 모듈화:

```typescript

// 슬라이스 패턴 예시

const useSearchSlice = () => useStore((state) => ({

  query: state.query,

  results: state.results,

  setQuery: state.setQuery,

  setResults: state.setResults,

}));

```

  

### 6.3 미들웨어 추가 가능

- **persist**: 로컬 스토리지에 상태 저장

- **devtools**: Redux DevTools 연동

- **immer**: 불변성 관리

  

**예시:**

```typescript

import { persist, createJSONStorage } from 'zustand/middleware';

  

export const useStore = create(

  persist<StoreState>(

    (set) => ({

      // ...

    }),

    {

      name: 'search-storage',

      storage: createJSONStorage(() => localStorage),

    }

  )

);

```

  

---

  

## 7. 파일 생성 계획

  

### 7.1 생성할 파일 목록

  

1. **`app/store/types.ts`**

   - 타입 정의 파일

   - StoreState 인터페이스

   - 관련 타입들

  

2. **`app/store/index.ts`**

   - 단일 store 인스턴스

   - Zustand `create` 함수 사용

   - export `useStore` 훅

  

3. **`app/page.tsx` (수정)**

   - `useState` 제거

   - `useStore` 사용으로 전환

  

### 7.2 파일 구조 최종안

  

```

frontend/

├── app/

│   ├── store/

│   │   ├── index.ts        # ✅ 새로 생성

│   │   └── types.ts        # ✅ 새로 생성

│   ├── layout.tsx

│   ├── page.tsx            # 🔄 수정 필요

│   └── ...

├── package.json            # 🔄 zustand 추가 필요

└── ...

```

  

---

  

## 8. 체크리스트

  

### 구현 전

- [ ] `pnpm add zustand` 실행

- [ ] `app/store` 디렉토리 생성

  

### 구현 중

- [ ] `app/store/types.ts` 작성

- [ ] `app/store/index.ts` 작성 (단일 store)

- [ ] `app/page.tsx` 마이그레이션

- [ ] 타입 에러 확인

  

### 구현 후

- [ ] 기능 동작 확인

- [ ] 타입 안정성 확인

- [ ] 성능 최적화 (선택적 구독)

  

---

  

## 9. 참고사항

  

### Zustand 장점

- ✅ 간단한 API

- ✅ Provider 불필요 (기본적으로)

- ✅ TypeScript 지원 우수

- ✅ 작은 번들 사이즈

- ✅ 성능 최적화 (선택적 구독)

  

### 단일 Store 원칙

- ✅ 하나의 진실의 소스 (Single Source of Truth)

- ✅ 상태 관리 일관성

- ✅ 디버깅 용이

- ✅ 코드 구조 단순화

  

---

  

**작성일**: 2024

**프로젝트**: spring-server/frontend

**목표**: Zustand를 사용한 단일 Store 구현