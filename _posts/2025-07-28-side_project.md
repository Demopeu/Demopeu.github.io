---
layout: single
title: "[Side Project] 모노레포 환경 구축①: Turborepo 루트 세팅"
categories: side_project
tags:
  - turborepo
  - husky
  - prettier
image:
  path: https://Demopeu.github.io/images/logo/SAGUK.png
  alt: "SAGUK"
  thumbnail: true
toc: true
author_profile: false
sidebar:
  nav: "counts"
use_math: true
---

![SAGUK](https://Demopeu.github.io/images/logo/SAGUK.png)

## ✅ 모노레포 환경 구축

이번에 개인 사이드 프로젝트로 모노레포를 사용하여 단순 api를 위한 건 nextjs로, 사용자를 위한 React Native 앱을 만드려고 한다.

이전에 `Turborepo`를 사용해본 적이 있는데, 제대로 이해하지 못한 거 같아, 정리하면서 사용해보려고 한다.

설치 이전에, 이번에 pnpm 버전을 10.13.1로 업그레이드 했다.

### 🙋‍♂️ 내가 Turborepo를 사용하는 이유

Git Organizaition이나 프로젝트 2개를 올려서 관리하기 번거롭다고 생각하여 모노레포 구조를 선택하였다.

그런데, pnpm으로 워크스페이스 설정하기, 공통 config 관리하기 등등 초기 설정부터 번거로운게 너무 많았다.

그리고 단순 api 용 nextjs 앱 배포할 때, 바로 Vercel 세팅하고 배포까지 가능한 Turborepo를 선택했다.

## 1. Turborepo 설치

```bash
pnpm dlx create-turbo@latest
```

https://turborepo.com/docs/getting-started 참고

## 2. turbo.json 파일 추가

```json
{
  "$schema": "https://turborepo.com/schema.json",
  "ui": "tui",
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
      "inputs": ["$TURBO_DEFAULT$", ".env*"],
      "outputs": ["dist/**", ".next/**", "!.next/cache/**"]
    },
    "lint": {
      "dependsOn": ["^lint"]
    },
    "check-types": {
      "dependsOn": ["^check-types"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "test": {
      "dependsOn": ["^test"],
      "outputs": []
    },
    "start": {
      "dependsOn": ["^start"],
      "outputs": []
    }
  }
}
```

### ⚙️ `$TURBO_DEFAULT$`의 역할과 변경 감지

inputs에 `$TURBO_DEFAULT$`를 포함하면, Turborepo는 해당 패키지 내의 `.gitignore`에 명시되지 않은 모든 파일을 자동으로 변경 감지 대상으로 설정한다.

이렇게 하면, `["app/", "components/"]` 같은 설정을 일일이 추가할 필요가 없어진다.

그리고 turbo run build를 실행할 때, pnpm-workspace.yaml에 정의된 모든 워크스페이스(apps/_, packages/_)를 순회하며 각 패키지의 build 스크립트를 실행한다.

이후, outputs에 캐싱 결과물을 명시하기만 하면 된다. 그런데 이번 Expo(React Native)는 빌드 방식이 달라서 따로 캐싱 안해도 된다는데 이건 추후에 변경할 수 있을 듯. 아직 React Native를 사용 안해봐서 모름.

### ⚙️ Turborepo의 빌드 순서

빌드 순서는 개발자가 수동으로 정하는 게 아니라, `package.json`의 의존성(dependencies)과 `turbo.json`의 `dependsOn` 설정을 통해 자동으로 결정되게 설계되어 있다.

apps/\**/package.json 파일의 dependencies에 workspace: *로 명시하면 의존성 그래프를 자동으로 생성한다.

이후, dependsOn: ["^build"]로 설정하면, 어떤 패키지의 build를 실행하기 전에, 그 패키지가 의존하는(package.json에 명시된) 모든 패키지들의 build를 먼저 실행하도록 자동으로 설정된다.

## 3. package.json script 설정

```json
 "scripts": {
    "graph": "turbo run build lint check-types --graph=graph.png",
    "check-before-commit": "turbo run lint check-types --filter=...[origin/dev]",
    "check-before-push": "turbo run build --filter=...[origin/dev]",
    "prepare": "husky install"
  },
```

**`graph`**: `turbo run build lint check-types --graph=graph.png`를 실행하여 `build`, `lint`, `check-types` 태스크의 의존성 그래프를 시각적으로 보기 위해 `graph.png` 파일로 생성하도록 했다.

**`check-before-commit`**: `turbo run lint check-types --filter=...[origin/dev]`를 실행하여 현재 브랜치에서 `origin/dev` 브랜치와 비교하여 변경된 패키지에 대해서만 `lint`와
`check-types`를 실행하도록 했다.

**`check-before-push`**: `turbo run build --filter=...[origin/dev]`를 실행하여 푸시하기 전에 변경된 패키지에 대해서만 `build`를 실행하도록 했다.

**`prepare`**: `husky install`을 실행하여 Git 훅을 설치하도록 했다. 이 스크립트는 `npm install` 또는 `pnpm install` 실행 시 자동으로 호출 시킴. 요즘에는 `husky install` 안쓰는 추세지만, 다른 곳에서 작업할때를 위해 해놓음.

### 🙋‍♂️ 동적으로 하지 않고 정적으로 하는 이유

--filter 말고 git diff --name-only HEAD^ 이런식으로 마지막 커밋과 비교하여 변경된 파일들을 가져와서 찾을 수 있는데, 이건 리눅스 명령어라서 윈도우에서 실행하다가 오류가 너무 자주 발생함. 이번기회에 Mac 샀으니까 추후에 스크립트 짜서 다시 도전해볼 듯.

## 4. husky, prettier, commitlint 설치

모노레포 환경에서 코드 품질과 커밋 메시지 컨벤션을 일관되게 유지하는 것은 매우 중요하다.(물론 혼자하지만...) 이를 위해 `husky`, `prettier`, `commitlint`를 도입하였다.

### ⚙️ commit-msg

```
#!/bin/sh
npx commitlint --edit "$1"
```

### ⚙️ pre-commit

```
#!/bin/sh

echo "
██████╗ ███████╗███╗   ███╗ ██████╗ ██████╗ ███████╗██╗   ██╗
██╔══██╗██╔════╝████╗ ████║██╔═══██╗██╔══██╗██╔════╝██║   ██║
██║  ██║█████╗  ██╔████╔██║██║   ██║██████╔╝█████╗  ██║   ██║
██║  ██║██╔══╝  ██║╚██╔╝██║██║   ██║██╔═══╝ ██╔══╝  ██║   ██║
██████╔╝███████╗██║ ╚═╝ ██║╚██████╔╝██║     ███████╗╚██████╔╝
╚═════╝ ╚══════╝╚═╝     ╚═╝ ╚═════╝ ╚═╝     ╚══════╝ ╚═════╝

🔍 변경된 파일에 대해 lint/type-check 실행 중...
"

pnpm run check-before-commit
RESULT=$?

if [ $RESULT -ne 0 ]; then
  echo ""
  echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
  echo "❌ 검사 실패! commit이 중단되었습니다."
  echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
  echo ""
  exit 1
fi

echo ""
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "✅ 검사 통과! commit이 계속 진행합니다."
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo ""
```

이런식으로 구성하였다.
