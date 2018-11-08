Webpack에 CSS 적용시, FOUC가 발생 할 수 있다.

FOUC에 대해 위키를 읽어보면 아래와 같이 정의되어있다.

> FOUC(Flash Of Unstyled Content)는 외부의 CSS가 불러오기 전에 잠시 스타일이 적용되지 않은 웹 페이지가 나타나는 현상

Webpack을 통해 css을 받을경우, 브라우저 상의 Flow를 확인해보면, HTML Document를 읽고, webpack을 통해 build된 js파일을 읽고 그걸 통해 다시 css파일들을 읽어오므로, 그 시간 사이에 CSS 깨짐 현상이 나타나는것은 어떻게 보면 당연하다고 생각 할 수도 있다.

이런 현상을 해결하기위해 Webpack에서는 **Extract Text Plugin**을 제공한다.
Extract Text Plugin의 역할은 webpack에서 번들링 할 때, 특정 텍스트를 추출하여 별도의 파일로 분리해주는 것이다. 이를 통해, CSS를 특정 파일로 추출해내고, 추출된 CSS파일을 html에 미리 추가해둔다면 FOUC를 방지 할 수 있을 것이다.

설정은 [깃헙 문서](https://github.com/webpack-contrib/extract-text-webpack-plugin)에 친절하게 설명되어있다.

이 글에서는 Extract Text Plugin 추가 과정에서 경험한 것들에 대해 공유한다.

```js
 plugins: [
	new ExtractTextPlugin({
		filename: '[name].css'
	})
  ]
```

위 코드와 같이 filename에 [name]을 사용하여 설정 할 경우 chunk name 별로 css파일이 생성된다. 여기까지는 좋다. 하지만 문제는 CommonsChunkPlugin을 같이 사용하게 되면서 발생하였다.

먼저 CommonsChunkPlugin을 사용한 이유는 npm을 통해 가져온 lib 모듈들은 수정될 가능성이 낮으므로, 개발할 때 작성한 source와 분리하여 bulid시 cache를 최대한 활용하기 위함이다.

```js
plugins: [
	new webpack.optimize.CommonsChunkPlugin({
	    name: 'vendor',
	    minChunks: function (module) {
	    	return module.context && module.context.indexOf('node_modules') !== -1
	    },
	    fileName: '[name]'
	})
  ]
```

위와 같이 사용하여 vendor.js를 별도로 분리하여 사용하고 있었다. 그런데 ExtractTextPlugin를 같이 사용하니, npm을 통해 받은 css들이 vondor.css 라는 파일로 별도로 분리가 되는것이다.

별 문제가 되지 않는다면 사용해도 무관하다.
하지만 기존 프로젝트에서 css가 모듈단위로 분리되어있었다면 좋았겠지만, 분리되어있지 않았으므로, css의 순서에 영향을 받게되었다.
그래서 NPM을 통해 받은 module들과 작성된 css사이의 순서가 보장되어야헸다. 즉 CommonsChunkPlugin 사용이 불가했다. 

해결방법은 아래와같이 css파일 관련 조건을 걸어주면된다.

```js
plugins: [
	new webpack.optimize.CommonsChunkPlugin({ // npm으로 받은 library는 vendor.js로 관리
	    name: 'vendor',
	    minChunks: function (module) {
            if(module.resource && (/^.*\.(css|scss)$/).test(module.resource)) {
	    	      return false;
	    	}
            
	    	return module.context && module.context.indexOf('node_modules') !== -1
	    },
	    fileName: '[name]'
	})
  ]
```

그 결과, css는 entry 별로 분리되어 파일이 생성되었지만, npm을 통해 불러온 모듈들이 함께 포함되며, 순서를 유지 할 수 있었다.

결국 반나절을 구글링하며 삽질했지만, 해결책은 [webpack문서 commonsChunkPlugin 설명](https://webpack.js.org/plugins/commons-chunk-plugin/)에 친절하게 써있었다...
관련 이슈가 많이 생길것 같지만 블로그 들에 공유된 것들이 거의 없는것 같아 공유하게 되었다.

문서를 먼저 잘 읽어야지..