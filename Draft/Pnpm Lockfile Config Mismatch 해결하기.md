---
title: "[[Pnpm Lockfile Config Mismatch 해결하기]]"
date: 2024-03-03 23:11 +0900
categories: "[post, error]"
tags: 
image:
---
새로운 프로젝트를 시작하면서 `CI`를 설정하던 중 아래와 같은 에러가 발생하였다.

![[pnpm-locfile-config-mismatch.webp]]

## Error

> [!Error]
> **ERR_PNPM_LOCKFILE_CONFIG_MISMATCH**  
> Cannot proceed with the frozen installation. The current `"settings.autoInstallPeers"` configuration doesn't match the value found in the lockfile
> 
> Update your lockfile using "pnpm install --no-frozen-lockfile"

에러 로그에 따르면 현재 `"settings.autoInstallPeers"` 설정이 `lockfile`에 있는 정보와 다르기 때문에 `frozen install`을 할 수 없어 발생한 문제처럼 보인다. 로그에서는 문제 해결을 위해 `pnpm install --no-frozen-lockfile` 을 이용하여 `lockfile`을 업데이트할 것을 권하고 있다.

## Approach
에러 로그에서 제안하고 있는 `pnpm install --no-frozen-lockfile`를 사용하여 `lockfile`을 업데이트 하기에 앞서, 로그에서 이야기 하고 있는 `frozen installation`이 무엇인지 찾아보았다. 

> [!info] [Frozen Installation](https://pnpm.io/next/cli/install#--frozen-lockfile)
> `--frozen-lockfile` 옵션을 활성화 하면 pnpm이 lockfile을 생성하지 않으며 lockfile의 내용이 명세서와 맞지 않거나 업데이트가 필요하거나 혹은 lockfile이 존재하지 않는 경우 설치에 실패한다. pnpm은 `CI` 환경에서 기본으로 이 옵션을 활성화한다.
> 
> 즉 여기에서 이야기하는 `frozen installation` 이란 lockfile을 생성하지 않고 의존성을 설치하는 것을 의미한다.

### Non Frozen Installation
위의 정보를 바탕으로 다시 에러를 살펴보면 다음과 같이 생각할 수 있다.

> [!note]
> `CI` 환경에서 `pnpm`은 `frozen installation`을 진행하였고 `"settings.autoInstallPeers"` 설정이 `lockfile`에 있는 설정과 다르기 때문에 에러가 발생하였다. 따라서 `Non frozen installation`을 진행하여 새로운 `lockfile`을 생성할 것을 권하고 있는 것이다.

`CI`에서 `pnpm install` 스크립트를 `pnpm install --no-frozen-lockfile`로 업데이트하여 의존성 설치를 진행하면 에러가 해결될 것 처럼 보인다. 하지만 이 방법에는 한 가지 의문이 남아있다. 

> [!question]
"이전까지 동일한 스크립트를 사용하면서 문제가 발생하지 않았었는데 왜 갑자기 에러가 생기게 된 걸까?"

이전 까지 문제 없이 해당 `CI`를 잘 사용하고 있었기 때문에 갑자기 스크립트에 문제가 있다고 보기는 어려웠다. 따라서 기존 `CI` 스크립트를 수정하는 것이 아닌 다른 방법을 찾아보게 되었다.

### Pnpm Issues
다른 방법을 찾아보던 중 `pnpm` 이슈에 있는 아래의 글을 통해 보다 명확한 이유와 해결방법을 알 수 있었다.

> [!note]
> [Getting a lockfile config mismatch after upgrading to version 8.6.1 #6649](https://github.com/pnpm/pnpm/issues/6649)

해당 이슈에 따르면 `pnpm`의 버전을 `8.6.1`로 업그레이드 한 후에 나와 동일한 문제가 발생하였다고 한다. 

`pnpm`의 메인테이너 중 한 명인 `zkochan`의 코멘트에 따르면 이 상황이 pnpm의 v7과 v8을 동시에 사용하려고 했기 때문에 발생한 문제











