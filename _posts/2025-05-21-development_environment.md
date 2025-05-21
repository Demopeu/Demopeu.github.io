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

![HUSKY](https://Demopeu.github.io/images/logo/HUSKY.png)

## ✅ Husky + Commitlint 수동 설정 가이드

{:.lead}
Git 커밋 메시지의 일관성을 유지하기 위해 `Husky`와 `Commitlint`를 **CLI 없이 수동으로 설정**하는 방법을 소개하려고 한다.

---

## 1. `.husky` 폴더에 훅 파일 수동 작성

### `.husky/commit-msg`

{% highlight sh %}
#!/bin/sh
npx commitlint --edit "$1"
{% endhighlight %}

### `.husky/pre-commit`

{% highlight sh %}
#!/bin/sh
pnpm run check-before-commit
{% endhighlight %}

---

## 2. 실행 권한 부여

{% highlight bash %}
chmod +x .husky/commit-msg
chmod +x .husky/pre-commit
{% endhighlight %}

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

🧩 터보레포 환경에서는 루트에 설치해야 함:

```bash
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

- 💡 위 규칙은 팀 상황에 맞게 커스터마이징 가능

---

## 6. Git 훅 연결 (수동 방식)

{% highlight bash %}
echo '#!/bin/sh' > .git/hooks/commit-msg
echo 'exec .husky/commit-msg "$@"' >> .git/hooks/commit-msg
chmod +x .git/hooks/commit-msg
{% endhighlight %}

---

## 7. 커밋 메시지 테스트

```bash
git add .

git commit -m "잘못된 메시지"  # ❌ 막혀야 정상
git commit -m "feat: 허스키 설정"  # ✅ 통과
```

---

## 8. 📌 마무리

- pnpm husky install 없이도 수동 연결로 동작 가능(install을 8버전부터 지원하지 않음)

- .git/hooks/commit-msg는 반드시 .husky/commit-msg를 실행하도록 설정
