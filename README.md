# 3.2.2-Creating-a-ggplot
Materi 3-Data visualisation, 3.2.2 Creating a ggplot

<h1><span class="header-section-number">3</span>&nbsp;Data visualisation</h1>
<div id="introduction-1" class="section level2">
<h2><span class="header-section-number">3.1</span>&nbsp;Introduction</h2>
<blockquote>
<p>&ldquo;The simple graph has brought more information to the data analyst&rsquo;s mind than any other device.&rdquo; &mdash; John Tukey</p>
</blockquote>
<p>This chapter will teach you how to visualise your data using ggplot2. R has several systems for making graphs, but ggplot2 is one of the most elegant and most versatile. ggplot2 implements the&nbsp;<strong>grammar of graphics</strong>, a coherent system for describing and building graphs. With ggplot2, you can do more faster by learning one system and applying it in many places.</p>
<p>If you&rsquo;d like to learn more about the theoretical underpinnings of ggplot2 before you start, I&rsquo;d recommend reading &ldquo;The Layered Grammar of Graphics&rdquo;,&nbsp;<a class="uri" href="http://vita.had.co.nz/papers/layered-grammar.pdf">http://vita.had.co.nz/papers/layered-grammar.pdf</a>.</p>
<div id="prerequisites-1" class="section level3">
<h3><span class="header-section-number">3.1.1</span>&nbsp;Prerequisites</h3>
<p>This chapter focusses on ggplot2, one of the core members of the tidyverse. To access the datasets, help pages, and functions that we will use in this chapter, load the tidyverse by running this code:</p>
<div id="cb7" class="sourceCode">
<pre class="downlit sourceCode r"><code class="sourceCode R"><span class="kw"><a href="https://rdrr.io/r/base/library.html">library</a></span><span class="op">(</span><span class="va"><a href="http://tidyverse.tidyverse.org/">tidyverse</a></span><span class="op">)</span>
<span class="co">#&gt; ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──</span>
<span class="co">#&gt; ✔ ggplot2 3.3.2     ✔ purrr   0.3.4</span>
<span class="co">#&gt; ✔ tibble  3.0.3     ✔ dplyr   1.0.2</span>
<span class="co">#&gt; ✔ tidyr   1.1.2     ✔ stringr 1.4.0</span>
<span class="co">#&gt; ✔ readr   1.4.0     ✔ forcats 0.5.0</span>
<span class="co">#&gt; ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──</span>
<span class="co">#&gt; ✖ dplyr::filter() masks stats::filter()</span>
<span class="co">#&gt; ✖ dplyr::lag()    masks stats::lag()</span></code></pre>
<div class="copy"><button class="btn btn-outline-primary btn-copy" title="" type="button" data-toggle="popover" data-placement="top" data-trigger="hover" data-original-title="Copy to clipboard">Copy</button></div>
</div>
<p>That one line of code loads the core tidyverse; packages which you will use in almost every data analysis. It also tells you which functions from the tidyverse conflict with functions in base R (or from other packages you might have loaded).</p>
<p>If you run this code and get the error message &ldquo;there is no package called &lsquo;tidyverse&rsquo;&rdquo;, you&rsquo;ll need to first install it, then run&nbsp;<code><a href="https://rdrr.io/r/base/library.html">library()</a></code>&nbsp;once again.</p>
<div id="cb8" class="sourceCode">
<pre class="downlit sourceCode r"><code class="sourceCode R"><span class="fu"><a href="https://rdrr.io/r/utils/install.packages.html">install.packages</a></span><span class="op">(</span><span class="st">"tidyverse"</span><span class="op">)</span>
<span class="kw"><a href="https://rdrr.io/r/base/library.html">library</a></span><span class="op">(</span><span class="va"><a href="http://tidyverse.tidyverse.org/">tidyverse</a></span><span class="op">)</span></code></pre>
<div class="copy"><button class="btn btn-outline-primary btn-copy" title="" type="button" data-toggle="popover" data-placement="top" data-trigger="hover" data-original-title="Copy to clipboard">Copy</button></div>
</div>
<p>You only need to install a package once, but you need to reload it every time you start a new session.</p>
<p>If we need to be explicit about where a function (or dataset) comes from, we&rsquo;ll use the special form&nbsp;<code>package::function()</code>. For example,&nbsp;<code><a href="https://ggplot2.tidyverse.org/reference/ggplot.html">ggplot2::ggplot()</a></code>&nbsp;tells you explicitly that we&rsquo;re using the&nbsp;<code>ggplot()</code>&nbsp;function from the ggplot2 package.</p>
</div>
</div>
<div id="first-steps" class="section level2">
<h2><span class="header-section-number">3.2</span>&nbsp;First steps</h2>
<p>Let&rsquo;s use our first graph to answer a question: Do cars with big engines use more fuel than cars with small engines? You probably already have an answer, but try to make your answer precise. What does the relationship between engine size and fuel efficiency look like? Is it positive? Negative? Linear? Nonlinear?</p>
<div id="the-mpg-data-frame" class="section level3">
<h3><span class="header-section-number">3.2.1</span>&nbsp;The&nbsp;<code>mpg</code>&nbsp;data frame</h3>
<p>You can test your answer with the&nbsp;<code>mpg</code>&nbsp;<strong>data frame</strong>&nbsp;found in ggplot2 (aka&nbsp;<code><a href="https://ggplot2.tidyverse.org/reference/mpg.html">ggplot2::mpg</a></code>). A data frame is a rectangular collection of variables (in the columns) and observations (in the rows).&nbsp;<code>mpg</code>&nbsp;contains observations collected by the US Environmental Protection Agency on 38 models of car.</p>
<div id="cb9" class="sourceCode">
<pre class="downlit sourceCode r"><code class="sourceCode R"><span class="va">mpg</span>
<span class="co">#&gt; # A tibble: 234 x 11</span>
<span class="co">#&gt;   manufacturer model displ  year   cyl trans      drv     cty   hwy fl    class </span>
<span class="co">#&gt;                          </span>
<span class="co">#&gt; 1 audi         a4      1.8  1999     4 auto(l5)   f        18    29 p     compa&hellip;</span>
<span class="co">#&gt; 2 audi         a4      1.8  1999     4 manual(m5) f        21    29 p     compa&hellip;</span>
<span class="co">#&gt; 3 audi         a4      2    2008     4 manual(m6) f        20    31 p     compa&hellip;</span>
<span class="co">#&gt; 4 audi         a4      2    2008     4 auto(av)   f        21    30 p     compa&hellip;</span>
<span class="co">#&gt; 5 audi         a4      2.8  1999     6 auto(l5)   f        16    26 p     compa&hellip;</span>
<span class="co">#&gt; 6 audi         a4      2.8  1999     6 manual(m5) f        18    26 p     compa&hellip;</span>
<span class="co">#&gt; # &hellip; with 228 more rows</span></code></pre>
<div class="copy"><button class="btn btn-outline-primary btn-copy" title="" type="button" data-toggle="popover" data-placement="top" data-trigger="hover" data-original-title="Copy to clipboard">Copy</button></div>
</div>
<p>Among the variables in&nbsp;<code>mpg</code>&nbsp;are:</p>
<ol>
<li>
<p><code>displ</code>, a car&rsquo;s engine size, in litres.</p>
</li>
<li>
<p><code>hwy</code>, a car&rsquo;s fuel efficiency on the highway, in miles per gallon (mpg). A car with a low fuel efficiency consumes more fuel than a car with a high fuel efficiency when they travel the same distance.</p>
</li>
</ol>
<p>To learn more about&nbsp;<code>mpg</code>, open its help page by running&nbsp;<code>?mpg</code>.</p>
</div>
<div id="creating-a-ggplot" class="section level3">
<h3><span class="header-section-number">3.2.2</span>&nbsp;Creating a ggplot</h3>
<p>To plot&nbsp;<code>mpg</code>, run this code to put&nbsp;<code>displ</code>&nbsp;on the x-axis and&nbsp;<code>hwy</code>&nbsp;on the y-axis:</p>
<div id="cb10" class="sourceCode">
<pre class="downlit sourceCode r"><code class="sourceCode R"><span class="fu">ggplot</span><span class="op">(</span>data <span class="op">=</span> <span class="va">mpg</span><span class="op">)</span> <span class="op">+</span> 
  <span class="fu">geom_point</span><span class="op">(</span>mapping <span class="op">=</span> <span class="fu">aes</span><span class="op">(</span>x <span class="op">=</span> <span class="va">displ</span>, y <span class="op">=</span> <span class="va">hwy</span><span class="op">)</span><span class="op">)</span></code></pre>
<div class="copy"><button class="btn btn-outline-primary btn-copy" title="" type="button" data-toggle="popover" data-placement="top" data-trigger="hover" data-original-title="Copy to clipboard">Copy</button></div>
</div>
<div class="inline-figure"><img src="https://d33wubrfki0l68.cloudfront.net/91aebc6de4de928abc810433b752274ba6a46d58/a78b1/visualize_files/figure-html/unnamed-chunk-3-1.png" alt="" width="70%" /></div>
</div>
</div>
