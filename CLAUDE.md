# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Astro-based blog called "coininsight" built from the official Astro blog template. It's a static site generator focused on content collections with Markdown/MDX support, RSS feeds, and sitemap generation.

## Development Commands

```bash
npm run dev      # Start dev server at localhost:4321
npm run build    # Build production site to ./dist/
npm run preview  # Preview production build locally
npm run astro    # Run Astro CLI commands
```

## Architecture

### Content Collections System

The project uses Astro's Content Collections API (`src/content.config.ts`) for type-safe blog content management:

- Blog posts are stored in `src/content/blog/` as Markdown or MDX files
- Content schema is defined with Zod validation in `src/content.config.ts`
- Required frontmatter: `title`, `description`, `pubDate`
- Optional frontmatter: `updatedDate`, `heroImage`
- Access posts using `getCollection('blog')` from `astro:content`

### Routing & Pages

- File-based routing in `src/pages/`
- Dynamic blog post routes: `src/pages/blog/[...slug].astro` uses `getStaticPaths()` to generate routes from content collection
- RSS feed: `src/pages/rss.xml.js` (endpoint route)
- Blog listing: `src/pages/blog/index.astro`

### Global Configuration

- Site metadata in `src/consts.ts` (`SITE_TITLE`, `SITE_DESCRIPTION`)
- Astro config in `astro.config.mjs` - includes MDX and sitemap integrations
- Update `site` URL in `astro.config.mjs` before deployment

### Project Structure

```
src/
├── components/     # Reusable Astro components (BaseHead, Header, Footer, etc.)
├── content/
│   └── blog/      # Blog post content (Markdown/MDX)
├── layouts/       # Page layouts (BlogPost.astro)
├── pages/         # Routes (file-based routing)
└── styles/        # Global CSS
```

## TypeScript Configuration

- Uses strict Astro TypeScript config (`astro/tsconfigs/strict`)
- `strictNullChecks` enabled
- Type definitions auto-generated in `.astro/types.d.ts`

---

## 블로그 운영 규칙 (Blog Content Guidelines)

### 블로그 주제 및 목적

- **주제**: 암호화폐(코인) 및 블록체인 기술
- **목적**: SEO 최적화를 통한 수익화 블로그 운영
- **교육적 가치**: 독자들이 코인과 블록체인을 학습할 수 있는 양질의 콘텐츠 제공

### 콘텐츠 작성 기준

**"새 글 작성"** 명령이 들어오면 다음 기준에 따라 자동으로 `.md` 파일을 생성:

1. **글 길이**: 최소 4,000자 이상 (SEO 및 독자 만족도 고려)

2. **주제 선정**:
   - 매번 새로운 주제로 작성
   - 코인/블록체인 관련 다양한 토픽 다루기
   - 예시: DeFi, NFT, 스마트 컨트랙트, 특정 코인 분석, 블록체인 기술 설명, 투자 전략, 시장 동향 등

3. **작성 스타일**:
   - 자연스럽고 친근한 말투 사용 (AI 느낌 최소화)
   - 구어체와 문어체를 적절히 혼합
   - 독자와 소통하는 느낌의 글쓰기
   - 전문 용어는 쉬운 설명과 함께 제공
   - **중요**: 마크다운 볼드 표시(`**`)를 사용하지 않기
   - **강조 표시**: 중요한 키워드나 강조가 필요한 부분은 노란색 글자로 표시
     - 사용법: `<span style="color: #FFD700;">강조할 텍스트</span>`
     - 예시: 주요 코인 이름, 핵심 개념, 중요한 용어 등

4. **SEO 최적화**:
   - 검색 의도에 맞는 키워드 자연스럽게 배치
   - 명확한 제목과 부제목 구조 (H1, H2, H3 활용)
   - 메타 설명(description)에 핵심 키워드 포함
   - 내부 링크 및 외부 신뢰성 있는 자료 참조

5. **이미지 삽입**:
   - **필수**: 글마다 최소 1개 이상의 관련 이미지 포함
   - **출처**: Unsplash에서 관련 키워드로 검색하여 첨부
   - 이미지는 글 내용과 연관성 높은 것으로 선택
   - 예시 키워드: "cryptocurrency", "blockchain", "bitcoin", "ethereum", "crypto trading" 등

6. **교육적 내용**:
   - 단순 정보 전달을 넘어 학습 가치 제공
   - 복잡한 개념은 단계별로 설명
   - 실제 사례나 예시 활용
   - 독자가 실제로 활용할 수 있는 팁 포함

### 파일 생성 규칙

- 파일명: 날짜 기반 또는 주제 기반 (예: `defi-guide.md`, `bitcoin-halving-2024.md`)
- 저장 위치: `src/content/blog/`
- 필수 frontmatter:
  ```yaml
  ---
  title: "SEO 최적화된 제목"
  description: "검색엔진에 표시될 설명 (150-160자)"
  pubDate: 2025-12-08T12:00:00
  heroImage: "https://images.unsplash.com/photo-xxxxx"
  ---
  ```
- pubDate 설정 규칙:
  - 최신 글이 위로 정렬되도록 날짜와 시간을 함께 관리
  - 하루에 여러 글을 작성할 수 있으므로 시간을 포함한 형식 사용 (ISO 8601 형식)
  - 새 글을 작성할 때마다 이전 글보다 나중 시간 사용
  - 예: 1번 글(2025-12-08T10:00:00), 2번 글(2025-12-08T11:00:00), 3번 글(2025-12-08T12:00:00)...
  - 최근 작성된 글의 pubDate를 확인하고 그보다 1시간 뒤로 자동 설정

### 자동화 워크플로우

사용자가 **"새 글 작성"**이라고 입력하면:
1. 최근 작성된 글의 pubDate 확인 (가장 최신 날짜+시간 찾기)
2. 최근 작성된 글 주제 확인 (중복 방지)
3. 새로운 주제 선정
4. 4,000자 이상의 고품질 콘텐츠 작성
5. Unsplash에서 적절한 이미지 검색 및 삽입
6. SEO 최적화된 frontmatter 작성 (pubDate는 이전 글보다 +1시간)
7. `src/content/blog/` 디렉토리에 `.md` 파일 저장
