## 아토믹 디자인 패턴이란? (**Atomic Design Pattern)**

아토믹 디자인은 *화학적 관점* 에서 영감을 얻은 디자인 시스템이다.

➡️ 모든 것은 atom(원자)으로 구성되어있고 atom(원자)들이 서로 결합하여 molecule(분자)이 되고, molecule는 더 복잡한 organism(유기체)으로 결합하여 궁극적으로 모든 물질을 생성한다.

<br/>
<br/>

#### ![이미지3](3_image.png)

<br/>
<br/>


### ✅ 아토믹 디자인 도입 이유

- 재사용성을 높이기 위해
- 디자인의 통일감을 높이기 위해
- 코드의 일관성을 위해 (컴포넌트 구조 명확화)

<br/>
<br/>

### 1️⃣ Atoms
- 가장 작은 단위의 컴포넌트를 의미한다.

➡️ 리액트에서는 Label, Input, CheckBox, Select 와 같은 HTML의 태그같이 기능적으로 가장 작은 단위이다.

➡️ atom 그 자체로는 바로 사용하는 경우가 거의 없고, atom을 다른 atom과 결합한 molecule 혹은 organism 단위와 결합하여 사용한다.

###### ![이미지4](4_image.png)

<br/>
<br/>

```
// components/atoms/Text.jsx

const Text = ({ children, size = 16, color = 'black' }) => {
  const colorMap = {
    red: 'text-red-500',
    blue: 'text-blue-500',
    black: 'text-black',
  };

  const sizeMap = {
    14: 'text-sm',
    16: 'text-base',
    20: 'text-xl',
  };

  return <p className={`${colorMap[color]} ${sizeMap[size]}`}>{children}</p>;
};

export default Text;
```

<br/>
<br/>

```
// components/atoms/ButtonBase.jsx

const ButtonBase = ({ children, onClick, variant = 'primary' }) => {
  const variantStyles = {
    primary: 'bg-blue-500 text-white hover:bg-blue-600',
    secondary: 'bg-gray-200 text-black hover:bg-gray-300',
    danger: 'bg-red-500 text-white hover:bg-red-600',
    ghost: 'bg-white text-gray-800 border border-gray-300 hover:bg-gray-100',
  };

  return (
    <button
      onClick={onClick}
      className={`px-4 py-2 rounded ${variantStyles[variant]}`}
    >
      {children}
    </button>
  );
};

export default ButtonBase;
```

<br/>
<br/>
<br/>
<br/>

### 2️⃣ Molecules

- atom을 여러 개 조합한 컴포넌트이다.
<br/>
<br/>

➡️ 아래의 이미지처럼 input atom과 button atom을 결합하면, 하나의 search molecules가 만들어진다.

<br/>
<br/>

#### ![이미지5](5_image.png)

<br/>
<br/>

```
// components/molecules/TextWithButton.jsx

import Text from '../atoms/Text';
import ButtonBase from '../atoms/ButtonBase';

const TextWithButton = ({ label, buttonText, onClick }) => {
  return (
    <div className="flex items-center gap-4">
      <Text size={16} color="black">{label}</Text>
      <ButtonBase onClick={onClick} variant="primary">
        {buttonText}
      </ButtonBase>
    </div>
  );
};

export default TextWithButton;
```

<br/>
<br/>
<br/>


### 3️⃣ Organisms
- atom, molecule에 비해 좀 더 구체적으로 표현되고 context를 가지기 때문에 상대적으로 재사용성이 낮다.
- header에 logo(atom)과 navigation(molecule), search form(molecule)을 넣은 형태가 하나의 header organism이 된다.
<br/>
<br/>

#### ![이미지6](6_image.png)

<br/>
<br/>

```
// components/organisms/NavBar.jsx

import Image from '../atoms/Image';
import Text from '../atoms/Text';
import ButtonBase from '../atoms/ButtonBase';

const NavBar = () => {
  return (
    <nav className="flex justify-between items-center px-8 py-4 shadow-md bg-white">
      {/* 왼쪽: 로고 */}
      <Image src="/logo.png" alt="로고" />

      {/* 가운데: 메뉴 텍스트 버튼들 */}
      <div className="flex gap-6">
        <button onClick={() => alert('home 클릭')}>
          <Text size={16} color="black">home</Text>
        </button>
        <button onClick={() => alert('about 클릭')}>
          <Text size={16} color="black">about</Text>
        </button>
        <button onClick={() => alert('blog 클릭')}>
          <Text size={16} color="black">blog</Text>
        </button>
        <button onClick={() => alert('contact 클릭')}>
          <Text size={16} color="black">contact</Text>
        </button>
      </div>

      {/* 오른쪽: 텍스트 + 버튼 직접 조합 */}
      <div className="flex items-center gap-2">
        <Text size={16} color="black"> 검색어입력하세요 </Text>
        <ButtonBase onClick={() => alert('검색')} variant="primary">
          검색
        </ButtonBase>
      </div>
    </nav>
  );
};

export default NavBar;
```

<br/>
<br/>
<br/>
<br/>


### 4️⃣ Templates
- page를 만들 수 있도록 여러 개의 organism, molecule로 구성된 것이다.

➡️ 페이지를 만들기 직전 마지막 단계로, 실체 컨텐츠 없이 레이아웃 배치를 하는 와이어 프레임의 개념이다.
<br/>
<br/>

#### ![이미지7](7_image.png)

<br/>
<br/>
<br/>
<br/>

### 5️⃣ Pages
- 유저가 볼 수 있는 최종 단계로, 실제 컨텐츠들이 담겨있다. 

➡️ 앞에서 만든 작은 단위들이(atom,molecule,organism)이 유기적으로 이루어진 하나의 페이지이다.

<br/>
<br/>

#### ![이미지8](8_image.png)

<br/>
<br/>
<br/>
<br/>


### ✅ 아토믹 디자인의 단점

- 초반 진입장벽이 높다. 초기 디자인 및 설정 단계에서 시간이 오래걸린다.
- 디자인 확정 후에 수정 하려면 번거롭다. (props를 타고가서 atom부터 바꿔야함)
- typescript와 함께 사용시 타입 지정 문제로 인해 에러가 많이 생긴다.
    
    → 하지만 props를 내려주는 과정에서 엄격하게 관리가 되기 때문에 의도치않은 실수를 예방할 수 있다.
    
    *(장점이자 단점)*
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

---
<br/>

*참고자료*

[*https://medium.com/@yongholeeme/atomic-design-for-react-514660f93ba*](https://medium.com/@yongholeeme/atomic-design-for-react-514660f93ba)

[*https://fe-developers.kakaoent.com/2022/220505-how-page-part-use-atomic-design-system/*](https://fe-developers.kakaoent.com/2022/220505-how-page-part-use-atomic-design-system/)

[*https://atomicdesign.bradfrost.com/chapter-2/*](https://atomicdesign.bradfrost.com/chapter-2/)  
