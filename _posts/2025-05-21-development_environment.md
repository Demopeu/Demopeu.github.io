---
layout: single
title: "[FE]Husky + Commitlint 수동 설정 가이드 (Git 커밋 메시지 검사 자동화)"
categories: 개발환경
tags:
  - git
  - husky
  - 커밋컨벤션
  - FrontEnd
image:
  path: https://Demopeu.github.io/images/logo/HUSKY.png
  alt: "HUSKY"
  thumbnail: true
toc: true
author_profile: false
sidebar:
  nav: "counts"
use_math: true
---

![BAEKJOON](https://Demopeu.github.io/images/logo/HUSKY.png)

# ✅ Husky + Commitlint 수동 설정 가이드

Git 커밋 메시지의 일관성을 유지하기 위해 `Husky`와 `Commitlint`를 수동으로 설정 하는 방법을 소개하려고 한다.

---

## 1. `.husky` 폴더에 `commit-msg`와 `pre-commit` 수동 작성

### 📄 .husky/commit-msg

```sh
#!/bin/sh
npx commitlint --edit "$1"
```

### 📄 .husky/pre-commit

```sh
#!/bin/sh
pnpm run check-before-commit
```

---

## 2. 실행 권한 부여

```bash
chmod +x .husky/commit-msg
chmod +x .husky/pre-commit
```

---

## 3. package.json 수정

```json
"scripts": {
  "lint": "next lint",
  "typecheck": "tsc --noEmit",
  "check-before-commit": "pnpm lint && pnpm run typecheck"
}
```

---

## 4. 의존성 설치 (CLI 없이)

```bash
pnpm add -D husky @commitlint/cli @commitlint/config-conventional
```

```bash
# 터보레포 이용 시 상위 설치가 필요
pnpm add -D -w husky @commitlint/cli @commitlint/config-conventional
```

---

## 5. `commitlint.config.js` 설정 파일 생성

```js
module.exports = {
  extends: ["@commitlint/config-conventional"],
  rules: {
    "type-enum": [
      2,
      "always",
      ["feat", "fix", "docs", "refactor", "chore", "design", "hotfix"],
    ],
    "subject-case": [
      2,
      "never",
      ["sentence-case", "start-case", "pascal-case", "upper-case"],
    ],
    "subject-full-stop": [2, "never", "."],
    "header-max-length": [2, "always", 50],
  },
};
```

상황에 맞게 변형하면 된다.

---

## 6. Git 훅 연결 (수동 방식)

```bash
echo '#!/bin/sh' > .git/hooks/commit-msg
echo 'exec .husky/commit-msg "$@"' >> .git/hooks/commit-msg
chmod +x .git/hooks/commit-msg
```

---

## 7. 커밋 메시지 테스트

```bash
git add .

git commit -m "잘못된 메시지"  # ❌ 막혀야 정상
git commit -m "feat: 허스키 설정"  # ✅ 통과
```
