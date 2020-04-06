---
layout: post
title:  "Ubuntu에서 Jekyll 설치하기"
---

Ubuntu에서 Jekyll을 설치하고, 로컬서버에서 접속해본다.

## 우분투에서 Jekyll 설치

```bash
# ruby 설치
sudo apt-get install ruby-full build-essential zlib1g-dev

# .bashrc에 적용
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Jekyll 설치
gem install jekyll bundler
```

## Jekyll 사이트 생성
```bash
jekyll new myblog
```

## 사이트 빌드 & 로컬 서버 적용
```bash
cd myblog
bundle exec jekyll serve
```

## 접속
http://localhost:4000 에 접속한다.

## 참고사이트
- [Jekyll 빠른 시작](https://jekyllrb-ko.github.io/docs/)
- [Jekyll Quickstart](https://jekyllrb.com/docs/)
- [ubuntu에서 jekyll 설치](https://jekyllrb.com/docs/installation/ubuntu/)
