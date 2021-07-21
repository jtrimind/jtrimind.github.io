---

title:  "[Meson] 메뉴얼 - Build options"
---

# Build options

대부분의 빌드는 사용자가 설정할 수 있는 옵션이 필요하다.  
예를 들면, 프로그램은 빌드 타임에 선택할 수 있는 서로 다른 데이터 백엔드를 가질 수 있다.  
Meson은 이를 위해 옵션 정의 파일을 제공한다.  
`meson_options.txt`라는 이름을 가진 파일이 그것이며, 소스 트리의 루트에 위치한다.  
옵션파일의 예는 아래와 같다.  

```meson
option('someoption', type : 'string', value : 'optval', description : 'An option')
option('other_one', type : 'boolean', value : false)
option('combo_opt', type : 'combo', choices : ['one', 'two', 'three'], value : 'three')
option('integer_opt', type : 'integer', min : 0, max : 5, value : 3) # Since 0.45.0
option('free_array_opt', type : 'array', value : ['one', 'two'])  # Since 0.44.0
option('array_opt', type : 'array', choices : ['one', 'two', 'three'], value : ['one', 'two'])
option('some_feature', type : 'feature', value : 'enabled')  # Since 0.47.0
option('long_desc', type : 'string', value : 'optval',
       description : 'An option with a very long description' +
                     'that does something in a specific context') # Since 0.55.0
```

빌트인 옵션을 더 알고 싶다면 [빌트인 옵션](https://mesonbuild.com/Builtin-options.html)을 참고하라.  

## Build option types

모든 타입은 해당 옵션을 설명하기 위한 `description` 값을 설정할 수 있다.  
`description` 값이 설정되지 않았다면 옵션의 이름이 설정될 것이다.  

### String
문자열

### Boolean
`true`나 `false`

### Combo
`choices`에 설정된 리스트 중 하나

### Integer
`min`과 `max` 사이의 정수

### Arrays
string의 배열. `choices` 파라미터를 이용하여 가질 수 있는 값을 제한 가능.

### Features
`enabled`, `disabled`, `auto` 중 하나.  
`required` 키워드에 넣기 위한 것으로 `dependency()`, `find_library()`, `find_program()`, `add_languages()`에 사용할 수 있다.  

- `enabled`: `required : true`와 같음
- `auto`: `required : false`와 같음
- `disabled`: 디펜던시를 찾지 않고 'not-found'를 리턴

`get_option`을 사용하여 아래와 같이 사용 가능하다.  

```
d = dependency('foo', required : get_option('myfeature'))
if d.found()
  app = executable('myapp', 'main.c', dependencies : [d])
endif
```

feature의 값을 확인하기 위해서 boolean을 리턴하는 세 함수를 사용하면 된다.  

- .enabled()
- .disabled()
- .auto()

```
if get_option('myfeature').enabled()
  # ...
endif
```

## Using build options

```
optval = get_option('opt_name')
```

`get_option`을 이용하여 옵션의 값을 얻어올 수 있다.  
예를 들어 installation prefix를 얻기 위해서 아래와 같이 사용하면 된다.  

```
prefix = get_option('prefix')
```

Meson scripts에서 옵션을 변경하지 말고 `meson configure`을 통해 외부에서 옵션을 설정해야 한다.  
인자 없이 `meson configure`을 실행하면 설정할 수 있는 모든 옵션이 나타난다.  
`-D` 옵션을 통해 옵션 값을 바꿔줄 수 있다.  

```bash
$ meson configure -Doption=newvalue
```

## Yielding to superproject option
상위 프로젝트와 하위 프로젝트가 있을 때, `yield` 키워드를 이용하면 동일한 값을 가진 옵션을 사용할 수 있다.  
아래와 같은 옵션을 가진 프로젝트가 있다고 생각하자.  

```
option('some_option', type : 'string', value : 'value', yield : true)
```

이 프로젝트를 빌드한다면 기존과 같이 동작한다.  
하지만 이 프로젝트를 다른 프로젝트의 하위 프로젝트로 빌드한다면, `get_option`은 상위 프로젝트의 값을 반환한다.  
`yield`의 값이 `false`라면, `get_option`은 하위 프로젝트의 옵션 값을 리턴한다.  

## Built-in build options
[빌트인 옵션](https://mesonbuild.com/Builtin-options.html)은 많다.  
현재 빌트인 옵션 리스트를 얻고 싶다면 `meson configure`을 실행해라.  
