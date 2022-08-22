<script setup lang="ts">
</script>

<template>
  <div class="[&>h1]:text-3xl">
    <h1>流、元素与基本尺寸</h1>
    <div class="[&>p]:m-3 [&>p]:p-1 [&>p]:bg-pink-300 [&>p]:text-lg [&>ol]:p-3 [&>ol]:list-inside [&>ol]:list-decimal">
      <p>流</p>
      略...
      <p>盒子</p>
      <div>根据display的不同</div>
      <ol>
        <li>值为block的元素的盒子实际上是由外在的”块级盒子“和内在的”块级容器盒子“构成的</li>
        <li>值为inline-block的元素的盒子实际上是由外在的”内联盒子“和内在的”块级容器盒子“构成的</li>
        <li>值为inline的元素的盒子实际上是由外在的”内联盒子“和内在的”内联盒子“构成的</li>
      </ol>
      <div text-center inline-table w-128px ml-10px border="~ gray">
        <p table-cell>
          第一列
        </p>
        <p table-cell>
          第二列
        </p>
      </div>
      <a block p-3>我是一个块链接, 我的内部会自动流动 不用设置width</a>
      <p>
        外部尺寸与流体特性
      </p>
      <h6>正常流宽度</h6>
      <ol>
        <li>设置display: block,就没有必要设置width: 100%了</li>
        <li>三无准则：无宽度 无图片 无浮动。 外部尺寸的块级元素一旦设置的宽度，就失去了流动性</li>
      </ol>
      <h6>格式化宽度</h6>
      <ol>
        <li>仅出现在“绝对定位模型中”（position为absolute或fixed）</li>
        <li>默认情况下，绝对定位元素宽度表现为“包裹性”，宽度由内部尺寸决定，只有一个例外⬇️</li>
        <li>
          非替换元素：当<em>left/right或bottom/top</em>对立方位属性值同时存在时，元素的宽度表现为“格式化宽度”，宽度大小相对于最近的具有定位特性（position属性值不是static）的祖先元素计算
          <details inline>
            <div border w-100px h-100px relative>
              <div absolute h-full left-20px right-20px bg-gray-300>
                width: 100px - 20px - 20px
              </div>
            </div>
          </details>
        </li>
      </ol>
      <p>内部尺寸与流体特性</p>
      <div>
        快速判断元素使用的是否为”内部尺寸“，如果一个元素内部没有内容的时候宽度是0，那就是应用的”内部尺寸“
      </div>
      <ol>
        <li>
          包裹性（shrink-to-fit），包裹性和自适应性，自适应性：元素尺寸由内部元素决定，但永远小于“包含块”容器的尺寸（除非容器尺寸小于元素的“首选最小宽度”），换句话说就是“包裹性”元素冥冥之中有一个max-width: 100%罩着的感觉（便于理解可以这么解释，事实并不是这样）
          <details inline text-center w-240px>
            <button>按钮</button>
            <button>比较长的按钮</button>
            <button>这个按钮特别长这个按钮特别长这个按钮特别长这个按钮特别长</button>
            <div inline-block text-left>
              这个按钮特别长这个按钮特别长这个按钮特别长这个按钮特别长这个按钮特别长这个按钮特别长这个按钮特别长这个按钮特别长
            </div>
          </details>
          除了inline-block，浮动元素和绝对定位元素都具有“包裹性”
        </li>
        <li>
          首选最小宽度 如果外部容器width为0，内部的inline-block元素的宽度并不会为0，因为在CSS中，图片和文字的权重是远大于布局的，CSS的设计者不会让width: auto的时候宽度变为0点，此时就是“首选最小宽度”。东亚文字
          <details>
            比如中文，最小宽度为每个汉字的宽度
          </details>
          西方文字
          <details>
            由特定的连续英文字符单元决定，一般会终止于空格（普通空格）、短横线、问号以及其他非英文字符
          </details>
          如果想让英文字符和中文一样，每一个字符都用最小宽度单元，可以试试使用CSS中的word-break:break-all
        </li>
        <li>
          最大宽度 就是元素可以有的最大宽度，等同于“包裹性”元素设置了white-space: nowrap之后的宽度。如果内部没有块级元素或者块级元素没有设定宽度值，则“最大宽度”实际上是最大的连续内联盒子的宽度。
        </li>
      </ol>
      <p>width值作用的细节</p>
      <div w-180px h-150px p-30px m-30px border="30px">
        content
      </div>
      <div>
        CSS2.1的规范中：content box围绕着width和height给定的矩形，所以给定width: 100px, 是作用在content box上的
      </div>
      <p>简单而相对单纯的height: auto CSS默认流是水平方向的，宽度是稀缺的，高度是无限的</p>
      <div>height: auto也有外部尺寸特征，仅存于绝对定位模型中，也就是“格式化高度”，与“格式化宽度”类似</div>
      <p>关于height: 100%</p>
      <div>
        对于width，就算父元素width: auto, 子元素的百分比也是支持的，但是对于height属性，父元素设置了height: auto, 只要子元素在文档流中，其百分比值就完全被忽略了
        <details>
          比如张三在页面插入一个div，想满屏幕显示图片
          <pre>
          div {
          　  width: 100%;　 /* 这是多余的 */
          　  height: 100%;　 /* 这是无效的 */
          　  background: url(bg.jpg);
          }
          </pre>
          然后他发现这个div高度永远都是0，哪怕其父级body塞满了内容也是这样，其实他需要这么设置才行
          <pre>
          html, body {
            height: 100%;
          }
          </pre>
          仅仅设置body也是不行的，因为此时body也没有具体的高度值
          <pre>
          body {
            /* 子元素高度100%依旧无效 */
          }
          </pre>
        </details>
        <div>对于普通文档流中的元素，百分比高度想要起作用，其父级必须要有一个可以生效的高度值</div>
      </div>
      <p>为什么父级没有具体高度值的时候，height: 100%会失效</p>
      <div>
        因为浏览器的渲染原理是按照从上而下，自外向内的顺序渲染DOM的，先渲染父元素再渲染子元素，当渲染到父元素的时候，子元素的width:100%并没有渲染，宽度就是图片加文字内容的宽度；等渲染到文字这个子元素的时候，父元素宽度已经固定，此时的width:100%就是已经固定好的父元素的宽度。宽度不够怎么办？溢出就好了，overflow属性就是为此而生的。
      </div>
      <p>如何让元素支持height: 100%的效果</p>
      <ol>
        <li>设置显式的高度值，比如height: 300px或者可以生效的百分比值高度，比如比较常见的html, body { height: 100%; }</li>
        <li>
          使用绝对定位div { height: 100%; position: absolute; },即使祖先元素的height计算值为auto。
          需要注意的是，绝对定位元素的百分比计算和非绝对定位元素的百分比计算是有区别的：绝对定位的宽高百分比计算是相对于padding box的，非绝对定位的宽高百分比计算是相对于content box的
          <details>
            <div w="1/2">
              <div h-160px p-30px bg-pink-3 box-border>
                <div h-full bg-amber>
                  高度100px
                </div>
              </div>
              <div mt-30px relative h-160px p-30px bg-pink-3 box-border>
                <div absolute h-full w-full bg-amber>
                  高度160px
                </div>
              </div>
            </div>
          </details>
        </li>
      </ol>
      <p mt-36px>
        CSS min-width/max-width和min-height/max-height
      </p>
    </div>
  </div>
</template>
