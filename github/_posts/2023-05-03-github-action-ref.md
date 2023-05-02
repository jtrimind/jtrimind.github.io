---
title:  "[GitHub Actions] ref, ref_name, base_ref, head_ref"
modified_date: 2023-05-03
---

## ref, ref_name, base_ref, head_ref
`ref`, `ref_name`, `base_ref`, `head_ref`는 GitHub Actions에서 사용되는 변수들로, 워크플로우가 특정 이벤트에 의해 실행될 때 참조하는 브랜치나 태그와 관련된 정보를 제공합니다.  
`base_ref`와 `head_ref`는 `pull_request` 또는 `pull_request_target`인 경우에만 사용할 수 있습니다.

### `github.ref`
워크플로우 실행을 트리거한 브랜치(branch) 또는 태그(tag)의 **완전한 형식의 참조(ref)**입니다.  

- push: push에 의해 트리거된 워크플로우의 경우, 푸시된 브랜치 또는 태그 레퍼런스입니다. `refs/heads/<branch_name>` 형식입니다.
- pull_request: pull_request에 의해 트리거된 워크플로우의 경우, 풀 리퀘스트 병합 브랜치입니다. `refs/pull/<pr_number>/merge` 형식입니다.
- release: release에 의해 트리거된 워크플로우의 경우, 생성된 릴리스 태그입니다. `refs/tags/<tag_name>` 형식입니다.
- 다른 트리거의 경우, 워크플로우 실행을 트리거한 브랜치 또는 태그 참조입니다.

### `github.ref_name`
워크플로우 실행을 트리거한 브랜치 또는 태그의 **짧은 참조 이름**입니다.  
이 값은 GitHub에 표시된 브랜치 또는 태그 이름과 일치합니다.

### `github.base_ref`
워크플로우 실행에서 풀 리퀘스트의 `base_ref` 또는 대상 브랜치(target branch)입니다.  
이 속성은 워크플로 실행을 트리거하는 이벤트가 `pull_request` 또는 `pull_request_target`인 경우에만 사용할 수 있습니다.

### `github.head_ref`
워크플로 실행에서 풀 리퀘스트의 `head_ref` 또는 소스 브랜치(source branch)입니다.  
이 속성은 워크플로 실행을 트리거하는 이벤트가 `pull_request` 또는 `pull_request_target`인 경우에만 사용할 수 있습니다.

## 예시 1
`main` 브랜치에 `push`로 워크플로우가 트리거 되는 경우 

| 속성 이름          | 예시 |
|-------------------|------|
| `github.ref`      | `refs/heads/main` |
| `github.ref_name` | `main` |
| `github.head_ref` |  |
| `github.base_ref` |  |

## 예시 2
`feature` 브랜치에서 `main` 브랜치로 풀 리퀘스트를 생성하여 워크플로우가 트리거 되는 경우

| 속성 이름          | 예시 |
|-------------------|------|
| `github.ref`      | `refs/pull/42/merge` |
| `github.ref_name` | `42/merge` |
| `github.head_ref` | `feature` |
| `github.base_ref` | `main` |

## 참고
- [`github` context](https://docs.github.com/en/actions/learn-github-actions/contexts#github-context)
