---
title:  "[GitHub Actions] 환경 변수(env, Environment files)"
modified_date: 2023-05-03
---

## env
`env`는 워크플로우(workflow), 작업(job), 단계(step)에 대한 환경 변수를 정의하기 위해 사용되는 맵(map)입니다.  
`${{ env.<env_name> }}`의 형태로 사용하거나 `$<env_name>`의 형태로 사용할 수 있습니다.  

```yaml
env:
  SERVER: production
jobs:
  linux_job:
    runs-on: ubuntu-latest
    steps:
    - run: echo "SERVER: ${{ env.SERVER }}"
    - run: echo "SERVER: $SERVER
```

## 같은 이름의 환경 변수가 정의되는 경우
같은 이름으로 둘 이상의 환경 변수가 정의되어 있는 경우, GitHub는 가장 구체적인 변수를 사용합니다.  

```yaml
name: Hi Mascot
on: push
env:
  mascot: Mona
  super_duper_var: totally_awesome

jobs:
  windows_job:
    runs-on: windows-latest
    steps:
      - run: echo 'Hi ${{ env.mascot }}'  # Hi Mona
      - run: echo 'Hi ${{ env.mascot }}'  # Hi Octocat
        env:
          mascot: Octocat
  linux_job:
    runs-on: ubuntu-latest
    env:
      mascot: Tux
    steps:
      - run: echo 'Hi ${{ env.mascot }}'  # Hi Tux
      - run: echo 'Hi ${{ env.mascot }}'  # Hi Octocat
        env:
          mascot: Octocat
```

위의 예시를 보면 워크플로우에서의 `env.mascot`는 `Mona`입니다.  
만약 단계에서 `env.mascot`를 `Octocat`이라고 재정의하게 되면 해당 단계에서 `env.mascot`은 `Octocat`이 됩니다.
작업에서도 마찬가지로 `env.mascot`를 `Tux`로 재정의할 수 있습니다.

## Environment files
Environment files는 `$GITHUB_ENV`에 환경 변수를 저장하는 방법입니다.  
echo를 이용하여 간단하게 환경 변수를 설정하거나, curl을 통해 환경 변수 파일을 받는 경우에 사용되는 방법입니다.  

```yaml
steps:
  - name: Set the value
    id: step_one
    run: |
      echo "action_state=yellow" >> "$GITHUB_ENV"
  - name: Use the value
    id: step_two
    run: |
      echo "${{ env.action_state }}" # This will output 'yellow'
```

## 참고
- [`env` context](https://docs.github.com/en/actions/learn-github-actions/contexts#env-context)
- [`env`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#env)
- [`jobs.<job_id>.env`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idenv)
- [`jobs.<job_id>.steps[*].env`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsenv)
- [`Environment files`](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#environment-files)