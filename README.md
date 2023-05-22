# learn-jest-vite

# JEST REACT 18 feat.Vite

# JEST 개요

Jest는 React 애플리케이션을 테스트하기 위해 일반적으로 사용되는 JavaScript 테스팅 프레임워크입니다. 개발자들은 쉽게 단위 테스트를 작성하고 실행할 수 있습니다. 

# JEST 설정

## 1. 라이브러리 설치

<aside>
🔽 npm install -D @babel/preset-env @babel/preset-react @babel/plugin-transform-runtime @testing-library/jest-dom @testing-library/react @testing-library/user-event identity-obj-proxy jest jest-environment-jsdom

</aside>

- @babel/preset-env
대상 환경에 필요한 구문 변환(및 선택적으로 브라우저 폴리필)을 관리할 필요 없이 최신 JavaScript를 사용할 수 있는 스마트 사전 설정입니다.
- @babel/preset-react
React JSX 변환하는 함수 가져와 JSX 문법을 사용하기 위해 설치합니다.
- @babel/plugin-transform-runtime
바벨이 트랜스파일링하는 과정에서 폴리필이 필요한 부분을 내부 헬퍼 함수로 치환해줍니다.
헬퍼 함수를 재사용하여 코드 크기 절약합니다.
- @testing-library/jest-dom
Jest를 위한 DOM 요소 매쳐(matcher)들을 제공합니다.
- @testing-library/react
React 구성 요소 테스트하기 위한 Enzyme를 대체하는 가벼운 솔루션입니다.
- @testing-library/user-event
사용자의 이벤트를 발생시킬 수 있는 라이브러리입니다.
브라우저의 상호 작용 이벤트를 전달해 사용자 상호 작용을 시뮬레이션하는 라이브러리 입니다.
- identity-obj-proxy
임포트(import)한 CSS 모듈 등을 목(mock) 데이터로 사용할 수 있게 도와주는 라이브러리입니다.
- jest
테스트 프레임워크입니다.
- jest-environment-jsdom
Jest 버전 28 이상의 경우 패키지 크기를 줄이기 위해 jest-environment-jsdom을 기본 jest설치에서 제거되었습니다.
기본적으로 jest는 노드 환경을 사용하기에 브라우저 환경을 위한 모든 테스트를 지원하지 않으므로 이러한 유형의 UI 테스트를 지원하기 위한 브라우저 환경 구현입니다.

## 2. <root>.babelrc

- presets - 목적에 맞게 여러 개의 플러그인들을 모아놓은 프리셋들을 추가합니다.
- plugins - 실제 변환 작업을 처리하는 플러그인들을 추가합니다.

```jsx
{
	"env": {
		"test": {
			"presets": [
				"@babel/preset-env",
				[
					"@babel/preset-react",
					{
						"runtime": "automatic"
					}
				]
			],
			"plugins": [
				"@babel/plugin-transform-runtime"
			]
		}
	}
}
```

## 3. <root>jest.config.js

- testEnvironment
테스트 환경을 지정합니다. 기본 값은 "node" 입니다.
- moduleNameMapper
이미지, 스타일 같은 리소스들에 대한 스터빙(stubbing)을 처리할 모듈을 지정합니다.
    - Stubbing
        
        Stub - [https://velog.io/@onikss793/Basic-Test-Jest#stub-mock](https://velog.io/@onikss793/Basic-Test-Jest#stub-mock)
        stub은 아직 준비되지 못한 코드를 미리 정해진 답변으로 반환할 수 있도록 하는 매커니즘이다.
        가짜 객체가 실제로 동작하는 것처럼 보이게 만들어 놓은 객체로 호출자를 실제 구현물로부터 격리시켜 독립적인 테스트를 진행할 수 있도록 한다.
        주로 아직 구현이 되지 않은 함수나, 라이브러리에서 제공하는 함수를 사용하고자 할 때, 혹은 복잡한 논리 흐름을 가져 테스트를 단순화하고 싶을 때, 의존성을 가지는 유닛의 응답을 모사혀여 독립적인 테스트를 수행하고자 할 때 사용된다.
        stub을 사용하게 되면 아직 구현이 되지 않았더라도, interface만으로도 테스트하고 개발할 수 있고 테스트의 단순화를 통해 다양한 응답 결과에 대한 촘촘한 케이스 검사를 수행할 수 있다.
        
- setupFilesAfterEnv
테스트 코드가 실행되기 전에 테스팅 프레임워크 설정을 위한 코드를 수행시킬 모듈의 경로를 지정합니다.
- testMatch
테스트 대상 파일들의 경로들을 지정합니다.
- transformIgnorePatterns
변환 대상이 아닌 경로들을 지정합니다.

```jsx
module.exports = {
	testEnvironment: 'jsdom',
	moduleNameMapper: {
		'\\.(png|pdf|svg|jpg|jpeg)$': '<rootDir>/__mocks__/fileMock.js',
		'\\.(css|styl|less|sass|scss|svg)$': 'identity-obj-proxy',
	},
	setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
	testMatch: ['<rootDir>/**/*.test.(js|jsx|ts|tsx)'],
	transformIgnorePatterns: ['<rootDir>/node_modules/', 'dist', 'build'],
}
```

## 4. <root>jest.setup.js

테스팅 프레임워크 설정을 위한 코드를 수행시킬 모듈

```jsx
import '@testing-library/jest-dom'
```

## 5. <root>/__mocks__/fileMock.js

정적 자산 관리(Handling Static Assets)로 CSS 및 이미지와 같은 파일을 정상적으로 처리하도록 구성합니다. 일반적으로 이러한 파일들은 테스트에 특별히 영향을 주지 않으므로 안전하게 목업하며 CSS 스냅샷 테스트를 지원합니다.

```jsx
module.exports = 'test-file-stub'
```

## 6. <root>package.json

type : module 제거 후 test script 작성합니다.

```jsx
{
  "name": "learn-jest",
  "private": true,
  "version": "0.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint src --ext js,jsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview",
    "test": "jest"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@babel/plugin-transform-runtime": "^7.21.4",
    "@babel/preset-env": "^7.21.5",
    "@babel/preset-react": "^7.18.6",
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^14.0.0",
    "@testing-library/user-event": "^14.4.3",
    "@types/react": "^18.0.28",
    "@types/react-dom": "^18.0.11",
    "@vitejs/plugin-react": "^4.0.0",
    "eslint": "^8.38.0",
    "eslint-plugin-react": "^7.32.2",
    "eslint-plugin-react-hooks": "^4.6.0",
    "eslint-plugin-react-refresh": "^0.3.4",
    "identity-obj-proxy": "^3.0.0",
    "jest": "^29.5.0",
    "jest-environment-jsdom": "^29.5.0",
    "vite": "^4.3.2"
  }
}
```

# Jest 테스트

<App /> 렌더링 이후 ‘Vite + React’ 텍스트를 가진 

```jsx
import { render, screen, waitFor } from '@testing-library/react'
import App from './App'
import userEvent from '@testing-library/user-event'

describe('App', () => {
	it('renders App', () => {
		render(<App />)

		expect(screen.getByText('Vite + React')).toBeInTheDocument()
	})

	const user = userEvent.setup();

	it('click count button', async () => {
		render(<App />)

		await userEvent.click(screen.getByText(/count is /i))
		await userEvent.click(screen.getByText(/count is /i))
		await userEvent.click(screen.getByText(/count is /i))
		await userEvent.click(screen.getByText(/count is /i))
		await userEvent.click(screen.getByText(/count is /i))
		await userEvent.click(screen.getByText(/count is /i))

		await waitFor(() => {
			expect(screen.getByText('count is 6')).toBeInTheDocument()
		})

	})
})
```

- describe(name, fn)
    
    테스트들을 ’App’으로 그룹화를 진행하였지만 필수 사항이 아니고 테스트들을 그룹으로 구성하는 경우에 사용할 수 있습니다. 그룹 없이 최상위 수준에서 직접 작성 가능합니다. 
    
- it(name, fn, timeout) / test(name, fn, timeout)
    
    테스트를 실행하는 메서드로 test의 별칭인 it을 사용하였습니다.
    
- render(ui, options?)
    
    DOM에 컴포넌트를 랜더링 해주는 메서드입니다. 
    
- expect(value)
    
    값을 테스트할 때 마다 사용되는데 스스로 호출하는 경우는 거의 없고 matcher 함수가 같이 붙습니다. 예를 들어 toBeInTheDocument() matcher 함수를 이용해 document에 요소가 있는지 확인합니다.
    
- screen
    
    render에서 나온 결과를 screen을 통해 쿼리 함수를 사용함으로써 접근하여 찾아낼 수 있다.
    쿼리 함수란 getByText(동일한 텍스트를 가진 요소 검색), getByLabelText(동일한 Label Text로 요소 검색) 등 페이지에서 요소를 찾기 위해 제공하는 방법입니다.
    
    - screen 사용 이유
        
        screen을 사용하지 않고 getByText(’Vite + React’)를 사용해도 되지만 screen을 사용하면 필요한 쿼리를 추가/제거할 때 렌더 호출 구조를 더 이상 최신 상태로 유지할 필요가 없습니다. 유일한 예외는 사용자가 피해야 할 컨테이너 또는 기본 요소를 설정하는 경우입니다
        
- toBeInTheDocument()
    
    document에 요소가 있는지 여부를 확인할 수 있습니다.
    
- userEvent
    
    기본적으로 userEvent를 사용해도 되지만 내부적으로 호출한 setup으로 호출하여 사용하는 것이 버전 14로의 전환을 용이하게 하고 간단한 테스트를 작성하기 위해좋습니다.
    

# Jest 테스트 오류

1. Validation Error : Test environment jest-environment-jsdom cannot be found.

```jsx
● Validation Error:

  Test environment jest-environment-jsdom cannot be found. Make sure the testEnvironment configuration option points to an existing node module.

  Configuration Documentation:
  https://jestjs.io/docs/configuration

As of Jest 28 "jest-environment-jsdom" is no longer shipped by default, make sure to install it separately.

Process finished with exit code 1
```

- 해결 방법
    
    npm i jest-environment-jsdom -D 설치 (자세한 설명은 Jest 설정 1. 라이브러리 설치 확인)
    

> [https://junhyunny.github.io/react/vite/jest/react-test-environment/](https://junhyunny.github.io/react/vite/jest/react-test-environment/)
> 

> [https://stackoverflow.com/questions/69227566/consider-using-the-jsdom-test-environment](https://stackoverflow.com/questions/69227566/consider-using-the-jsdom-test-environment)
>
