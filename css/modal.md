# 纯 css 实现的 modal

codepen: <https://codepen.io/linghucq1/pen/bvpzgV>

以前只知道 a 标签可以跳转到当前页面的某一个 id 元素， 从而实现页面的滚动。今天才知道原来还有这么神奇的用途。

:target 伪类里的样式会在元素被匹配上时生效。如 https://abc.com#modal， 如果页面内有 id 为 modal 的元素，则该元素的 :target 样式就会生效。

让元素本身 display: none，给它的 :target display: block，就可以实现 modal 的隐藏与显示。

用 a 标签可以切换被 target 的元素，于是我们有了 modal without javascript。

html:
``` html
<a href="#modal">see detail</a>
<div id="modal">
  <a class="bg" href="#"></a>
  <div class="modal-box">
    <a class="close" href="#">&times;</a>
  </div>
</div>
```
less:
``` less
#modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  justify-content: center;
  align-items: center;
  display: none;
  
  &:target {
    display: flex;
  }
  
  .bg {
    position: absolute;
    z-index: 1;
    background-color: rgba(0,0,0,.5);
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
  }
  
  .modal-box {
    width: 70%;
    height: 70%;
    position: relative;
    z-index: 2;
    background-color: #fff;
    border-radius: 6px;
    display: flex;
    flex-direction: column;
    
    .close {
      width: 20px;
      height: 20px;
      opacity: 0.8;
      transition: all 200ms;
      font-size: 24px;
      font-weight: bold;
      text-decoration: none;
      color: #666;
      align-self: flex-end;
      
      &:hover {
        opacity: 1;
      }
    }
  }
}
```