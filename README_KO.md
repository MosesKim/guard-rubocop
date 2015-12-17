[![Gem Version](http://img.shields.io/gem/v/guard-rubocop.svg)](http://badge.fury.io/rb/guard-rubocop)
[![Dependency Status](http://img.shields.io/gemnasium/yujinakayama/guard-rubocop.svg)](https://gemnasium.com/yujinakayama/guard-rubocop)
[![Build Status](https://travis-ci.org/yujinakayama/guard-rubocop.svg?branch=master)](https://travis-ci.org/yujinakayama/guard-rubocop)
[![Coverage Status](http://img.shields.io/coveralls/yujinakayama/guard-rubocop/master.svg)](https://coveralls.io/r/yujinakayama/guard-rubocop)
[![Code Climate](http://img.shields.io/codeclimate/github/yujinakayama/guard-rubocop.svg)](https://codeclimate.com/github/yujinakayama/guard-rubocop)

# guard-rubocop

**guard-rubocop** 을 사용하면 파일이 수정되는 즉시 하면 [RuboCop](https://github.com/bbatsov/rubocop)에 의거해 Ruby code style을 확인할 수 있습니다.

이 버전은 MRI 1.9, 2.0, 2.1, JRuby 1.9 및 Rubinius에서 테스트 완료되었습니다.

## Installation

이후 단계를 진행하기 전 [Guard](https://github.com/guard/guard)가 설치되어 있어야 합니다.

`guard-rubocop`을 `Gemfile`에 추가합니다:

```ruby
group :development do
  gem 'guard-rubocop'
end
```

이후 다음을 실행합니다:

```sh
$ bundle install
```

혹은 다음 명령어를 사용해 직접 guard-rubocop을 실행할 수 있습니다:

```sh
$ gem install guard-rubocop
```

다음 명령어를 실행해 `Guardfile`에 기본 Guard::Rubocop 정의를 추가합니다:

```sh
$ guard init rubocop
```

## Usage

[Guard usage documentation](https://github.com/guard/guard#readme)을 참조합니다.

## Options

다음 명령어를 실행하여 `Guardfile`에 파라미터를 전달할 수 있습니다:

```ruby
guard :rubocop, all_on_start: false, cli: ['--format', 'clang', '--rails'] do
  # ...
end
```

### Available Options

```ruby
all_on_start: true     # Guard 시작시 모든 파일들을 확인합니다.
                       #   기본값: true
cli: '--rails'         # 임의의 RuboCop CLI 파라미터를 전달합니다.
                       # 하나의 어레이(array) 혹은 하나의 스트링(string)을 전달할 수 있습니다.
                       #   기본값: nil
hide_stdout: false      # 콘솔에 아무런 내용을 출력하지 않습니다(별도 파일에 출력하는 경우).
                       #   기본값: false
keep_failed: true      # Rubocop 확인이 성공할 때까지 실패한 파일들을 유지합니다.
                       #   기본값: true
notification: :failed  # 실행시마다 Growl 알림 메시지를 표시합니다.
                       #   true    - 항상 알림
                       #   false   - 항상 알리지 않음
                       #   :failed - 실패한 경우에만 알림
                       #   기본값: :failed
```

## Advanced Tips

[`guard-rspec`](https://github.com/guard/guard-rspec)과 같은 테스팅 Guard 플러그인을 `guard-rubocop`과 함께 TDD 방식(red-green-refactor 사이클)로 활용하는 경우, red-green 단계에서 Robocop이 출력하는 offenses report가 귀찮을 수 있습니다.

* Red-Green 단계에서는 클린 코드를 작성할 필요가 없습니다 - 테스트를 통과하는 코드를 작성하는 것에만 집중합니다. 즉 이 단계에서는 `guard-rpsec`만 실행하고 `guard-rubocop`은 실행히지 않아야 합니다.
* Refactor 단계에서는 모든 테스트가 통과하는 상태로 클린 코드를 작성해야 합니다. 따라서 이 단계에서는 `guard-rspec`와 `guard-rubocop`을 모두 실행해야 합니다.

위와 같은 경우, 다음과 같은 구조의 `Guardfile`을 유용하게 활용할 수 있습니다:

```ruby
# 아래 group을 사용해 RSpec이 실패하는 경우 Rubocop을 실행시키지 않도록 할 수 있습니다
group :red_green_refactor, halt_on_fail: true do
  guard :rspec do
    # ...
  end

  guard :rubocop do
    # ...
  end
end
```

참고: `guard-rspec` 4.2.3 이상의 버전을 사용하야 sepc 파일이 없는 경우 의도치 않게 RSpec이 실패하는 [버그](https://github.com/guard/guard-rspec/pull/234)를 회피할 수 있습니다.

## Contributing

1. 이 리파지토리를 fork합니다.
2. 작업용 feature 브랜치를 만듭니다 (`git checkout -b my-new-feature`)
3. 변경 내용을 커밋합니다 (`git commit -am 'Add some feature'`)
4. 변경 내용을 브랜치에 푸시합니다. (`git push origin my-new-feature`)
5. 새 풀 리퀘스트(Pull Request)를 생성합니다.

## Translations

1. [한국어](./README_KO.md)

## License

Copyright (c) 2013–2014 Yuji Nakayama

See the [LICENSE.txt](LICENSE.txt) for details.
