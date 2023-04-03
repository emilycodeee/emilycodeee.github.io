---
title: 【前端框架】Vue & React 初開發比較與心得
date: 2022-03-27 18:29:38
tags: ["vue", "react","前端"]
categories: ["Tech"]
---

**前言**

既先前以 React 開發個人專案後，最近因為新工作需要使用 Vue ，所以轉而學習 Vue 開發，在這篇文章中會記錄我所感受與理解的 React 與 Vue 之間的差別。

## 📌 Vue 與 React 的共通點

- 都是用於建立 UI 的 JavaScript 庫
- 都使用 Virtual Dom
- 都實踐 Component Base 的架構模式
- 都可以放入個別 HTML 檔案中，也可以用為更複雜的 Webpack 設定中的一個模組。
- 都是單向資訊流
- 都有個別的路由與狀態管理庫

其實上述幾點列出來，也是最近的學習感受，本質來說兩個框架幾乎是一樣的，只是差別在於兩個框架使用的設計思考。

> 在寫 Vue 的時候，每個組件就像在寫一個完整的 HTML + JavaScript + CSS。
> 在寫 React 的時候，其實很大程度會感覺自己就是在寫 JavaScript。

## 📌 Vue 與 React 核心思想的不同

Vue 主打靈活易用的漸進式框架，也就是希望降低前端開發的門檻，因此很多人說 Vue 是目前三大框架中學習曲線最平緩的。而 Vue 的作者尤雨溪也曾如此回答：

> 「Vue 使用的是 web 开发者更熟悉的模板与特性，Vue 的 API 跟传统 web 开发者熟悉的模板契合度更高，比如 Vue 的单文件组件是以模板 + JavaScript + CSS 的组合模式呈现，它跟 web 现有的 HTML、JavaScript、CSS 能够更好地配合。React 的特色在于函数式编程的理念和丰富的技术选型。Vue 比起 React 更容易被前端工程师接受，这是一个直观的感受；React 则更容易吸引在 FP 上持续走下去的开发者。」

Vue 原先的定位是盡可能降低前端開發的門檻，也因此可以從這個出發點理解為什麼 Vue 推崇所謂的`漸進式開發體驗`，對於一個初學前端開發的開發者而言，數據可變動、雙向綁定，每個組件都包含 html(template)、javascript、css 等，其實與既有的所謂`經典web開發方式`所差無幾，也就相對而言較好理解與學習。這個核心價值的開展也或許可以歸結到尤雨溪本身是獨立開發者。

React 早期的口號是 `Rethinking Best Practices`。開宗明義地表示要用更好的方式去顛覆前端開發（在當時 jQuery 稱霸的時空背景下，現在看來確實完美顛覆了）。而在 React 中推崇函式編程(FP)，其中包含數據不可變及單向資訊流等，在 Pure Function 的原則下，最大優點當然就是所謂的穩定性（沒有 side effect）及可測試／維護性（同樣的 input，必得到同樣的 output），而這也是為什麼 React 更適合大型、複雜應用的原因。

在寫 Vue 時，會有 template、css、script 三個區塊組合，就像我們最開始在學習網頁開發時的 html、css、js 一樣。在 Vue 的官方文件中，也提到如此的設計思想是為了擁抱經典的 Web 開發技術。

**Vue Component**

```html
<template>
  <form>
    <label>Email:</label>
    <input type="email" required v-model="email" />
    <label>Password:</label>
    <input type="password" required v-model="password" />
    <label>Role:</label>
  </form>
</template>

<script>
  export default {
    name: 'Form',
    data() {
      return {
        email: 'sss',
        password: '',
        role: '',
      }
    },
  }
</script>

<style>

  <!-- component style -->
</style>
```

相較 Vue 的經典，在 React 中，一切都是 JavaScript 為基底，就連 HTML 都以 JSX 來表達(HTML in JS)，尤其搭配上 styled-components 時，基本上就感覺自己從來沒離開過 JavaScript 。

**React Component**

```javascript
const Form = () => {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')

  return (
    <form>
      <label>Email:</label>
      <input type="email" required value={email} />
      <label>Password:</label>
      <input type="password" required value={password} />
      <label>Role:</label>
    </form>
  )
}
```

## 📌 狀態數據綁定的不同

**Vue 雙向綁定 v.s React 單向綁定**

或許這也可以說是兩者間最大的區別，Vue 提供反應式的數據，也就是當我們使用 v-model 將數據連動時，只要數據有所改動，介面就會自動更新，而在 React 中，我們則需要透過 `onChange` 或 `setState` 方法來完成這件事。Vue 的作者尤雨溪將這兩者說明為`Push-based` 和 `Pull-based`。

**Push-based：Vue**

改動數據後，數據本身會把這個改動推送出去，告知渲染系統進行畫面渲染。

**Pull-based：React**

我們需要額外主動去通知系統「你該進行渲染」，這時系統才會幫我們渲染畫面。

## 📌 監聽資料變化與渲染過程的不同

![](https://i.imgur.com/n8YtJI0.png)

**Vue**

- 依賴收集，自動優化，數據可變。
- 遞迴監聽 data 的所有屬性，可以直接修改。
- 當數據改變時，自動找到引用組件重新渲染。

**React**

- 基於資料狀態，手動優化，數據不可變。
- 需透過 onChange 或 setState 等方法進行修改
- 以組件為根目錄，預設為全部重新渲染。

在 React 當某個組件的狀態發生變化的時候，會以該組件為根，重新渲染整個子組件，因此在寫 React 時，我們常會思考是不是會造成不必要的子組件渲染，因此就需要在所有可能的地方使用 PureComponent，或是手動實現 shouldComponentUpdate 方法，但如此一來，我們也就必須保證該組件的整個子組件渲染輸出的影響都是由該組件的 props 所決定，不然就可能導致例外且難以察覺的渲染結果不一致，導致在組件優化的考量上，會耗費比較多心力。

但在 Vue 中，因為組件的依賴引用在渲染過程中是`自動追蹤`，所以系統精確知道哪些組件需要被重新渲染，我們可以簡單理解為在 Vue 中的每個組件都自動獲得 shouldComponentUpdate ，因此在 Vue 開發而言，這個特點讓開發者相較不需要考量這一類的效能優化問題，而能更加專注於開發工作。

更清楚地說 Vue 之所以可以更快地計算出 Virtual DOM 的差異，是因為它在渲染過程中，會追蹤每一個元件的依賴關係（透過 getter/setter 等），不需要重新渲染整個元件樹，而 React 則是透過引用比較的方式(diff)，因此如果開發不注意可能會導致大量且不必要的 VDOM 重新渲染，談到這裡可能會好奇，怎麼 React 不跟著監聽資料就好？

這就要回歸到最源頭的兩個框架設計理念的區別，Vue 資料的「可變」，與 React 資料的「不可變」，而這兩者也其實沒有真實所謂的優劣之分，Vue 在這點上可以說是開發更簡單，但 React 則是在建構大型應用的時候，能更好的維護。

## 📌 生命週期 hook

承上，Vue 中的組件也與 React 一樣，有所謂類似生命週期的方法，例如當組件就緒，掛載到頁面之前 (created)，掛載到頁面時(mounted)、狀態更新時(updated)、組件撤銷時(unmounted)等 hook，而兩者最大的差別在於先前說的，Vue 本身是具有反應系統的，也就是已經預設了 shouldComponentUpdate ，所以在 Vue 裡當然就不需要它。

**Vue Lifecycle**

![](https://i.imgur.com/LnpYA3U.png)

**React Lifecycle**

![](https://i.imgur.com/63RCuYp.png)

## 📌 diff 的算法不同

Vue 與 React 在實踐 Virtual Dom 的方法是相當類似的，都是基於以下兩點，將傳統 Diff 算法從 O(n³) 降為 O(n)。

- 不同的組件產生不同的 DOM 結構物件。
- 當 type 不相同時，對應 DOM 操作就是直接刪除舊的 DOM ，建新的 DOM。
- 同一層級的子節點透過唯一的 key 來做為區分基礎。

**React 的 Diff 算法核心實現**

1. 首先對新集合進行遍歷，for( name in nextChildren)
2. 通過唯一 key 來判斷老集合中是否存在相同的節點。如果沒有的話創建
3. 如果有的話，if (preChild === nextChild )
   - 在新集合中的位置和在舊集合中 lastIndex 進行比較
   - 如果 if (child.\_mountIndex < lastIndex) 進行移動操作，否則不進行移動操作
4. 如果遍歷的過程中發現在新集合中沒有，但在老集合中有的節點，會進行刪除操作

**Vue 的 Diff 算法核心實現**

updateChildren 是 vue diff 的核心，過程可以概括爲：

舊 children 和新 children 各有兩個頭尾的變量 StartIdx 和 EndIdx，它們的 2 個變量相互比較，一共有 4 種比較方式。

如果 4 種比較都沒匹配如果設置了 key，就會用 key 進行比較，在比較的過程中，變量會往中間靠，一旦 StartIdx > EndIdx 表明舊 children 和新 children 至少有一個已經遍歷完了，就會結束比較。

Vue 是基於 snabbdom 庫，採用雙端比較的 diff 算法，同時從新舊 children 的兩端開始進行比較，藉助 key 值找到可複用的節點，再進行相關操作。也就是說 Vue Diff 是採用`邊比較、邊更新`，而 React 則是使用 diff 對列保存需要更新那些 DOM ，`算出 patch 樹後，在統一操作批量更新 DOM`，兩相比較下， Vue 可以減少移動節點次數，減少不必要的性能損耗，更加的優雅。

## 📌 數據流向與傳遞不同 Event v.s Callback

![](https://i.imgur.com/yoe7ngJ.png)

**Vue**

- 父元件透過 props 向子元件傳遞資料或者 callback，但一般只傳遞資料。
- 子元件透過自定義事件(custom event)向父元件傳遞資料。（實現方式類似於父層監聽子層的事件變化）
- 以 provide/inject 來實現父元件向子元件跨多層級的資料傳遞。

**React**

- 與 Vue 一樣，父元件透過 props 向子元件傳遞資料或者 callback。
- 透過 context 進行跨層級的資料傳遞，和 provide/inject 作用差不多。
- React 本身並不支援自定義事件。

Vue 與 React 都有方式可以輾轉達到子層項父層傳遞訊息，但 Vue 傾向於自定義事件(custom event)傳遞，而 React 則是使用回調函式。

```javascript
// custom event emit 範例

//子
export default {
  data() {
   ...
  },
  props: ["delay"],
  methods: {
    stopTimer() {
      this.$emit("end", this.reactionTime);
	//定義 event名稱,放入要傳遞的變數
    },
  },
};

//父
<template>
  <Block v-if="isPlaying" :delay="delay" @end="stopGame" />
	//觸發custom event時，會觸發父層method
  <div v-if="showResult">spent {{ reactionTime }}</div>
</template>

<script>
import Block from "./components/Block.vue";
Block;

export default {
  name: "App",
  components: { Block },
  data() {
...
  },
  methods: {
    stopGame(time) {
	//透過event emit定義的custom event
	//在觸發時會自動傳入所傳遞的參數
      this.reactionTime = time;
      this.isPlaying = false;
      this.showResult = true;
    },
  },
};
</script>
```

## 📌 事件機制的不同

**Vue**

- 原生事件使用標準 web 事件，（例 CustomEvent 也是其一種）。
- 具有自定義事件(custom event)，讓父組件可以實踐對子組件的監聽。
- 利用 snabbdom 庫的模組插件。

**React**

原生事件經過包裝，所有事件會先冒泡到頂層 document 監聽後，才合成事件向下傳遞，因此可以跨端使用事件機制，而不是經過 web dom 的強綁定。父子間都是透過 props 做為資料傳遞，並無向上傳遞。

## 📌 框架本質的不同

Vue 本質是 MVVM 框架（漸進式框架），由 MVC 發展而來；
React 是前端元件化框架，由後端元件化發展而來。

**個人感受小結**

**Vue 的優勢**

- 更快的渲染速度和更小的體積
- 簡單的語法及專案建立，尤其透過 vue cli 可以依專案快速設定細節，在建立時更為便利且快速。
- 響應式的資料狀態，開發更加快速便利

**React 的優勢**

- 適用於 Web 端和原生 App
- 適用於大型應用和更好的可測試/維護性
- 更大的生態圈帶來的更多支援和工具

其實當時在學習 React 時，一開始確實花了一些時間理解，但其實並沒有感受太大的進入障礙，可能很大一部分原因跟我個人很喜歡 JS 有關，再加上是以 React 的 Function Component 寫法開發，理解基本的 hook 如何使用後，就算不看文檔也可以順利開發，不會有太大的問題。

但進入 Vue ，因為有各種各樣的 api 操作，雖然起手簡單，但要能夠熟練以至於提高開發效率，過程需要需要不斷翻看文檔明白各項配置才真的能上手。而相較於此，React 沒有所謂這些林林總總的 api，因此學習起來，反而感覺 React 像是一張可以自由揮灑的白紙，而 Vue 則像是積木，你必須知道各種 api 的使用方式，知道哪裡該卡哪裡，才可以將整個畫面構件出來。

那我比較喜歡哪一種呢？
老實說我還是喜歡 React 。但 Vue 的靈活及相較於 React 的手動操作，Vue 自動響應的變化，還是令人驚嘆尤雨溪這位大神大概真的是神吧！

#### 參考資源

- [Vue : Comparison with Other Frameworks](https://cn.vuejs.org/v2/guide/comparison.html)
- [Vue 作者尤雨溪：以匠人的态度不断打磨完善 Vue](https://zhuanlan.zhihu.com/p/108899766)
- [Vue 與 React 的區別和優勢對比](https://www.796t.com/article.php?id=191922)
- [React 與 Vue — 有了 jQuery 為什麼要有 xxx？](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/748175/#outline__2)

#### 延伸閱讀

- [React Patterns](https://reactpatterns.cn/)
- [解析 snabbdom 源码，教你实现精简的 Virtual DOM 库](https://github.com/creeperyang/blog/issues/33)
