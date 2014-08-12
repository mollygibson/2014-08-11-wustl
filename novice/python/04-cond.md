---
layout: lesson
root: ../..
---

## Making Choices


<div class="">
<p>Our previous lessons have shown us how to manipulate data, define our own functions, and repeat things. However, the programs we have written so far always do the same things, regardless of what data they're given. We want programs to make choices based on the values they are manipulating</p>
</div>


<div class="objectives">
<h4 id="objectives">Objectives</h4>
<ul>
<li>Explain the similarities and differences between tuples and lists.</li>
<li>Write conditional statements including <code>if</code>, <code>elif</code>, and <code>else</code> branches.</li>
<li>Correctly evaluate expressions containing <code>and</code> and <code>or</code>.</li>
<li>Correctly write and interpret code containing nested loops and conditionals.</li>
<li>Explain the advantages of putting frequently-modified code in a function.</li>
</ul>
</div><div class="out">



### Conditionals


<div class="">
<p>The tool Python gives us for making choices	 is called a <a href="../../gloss.html#conditional-statement">conditional statement</a>, and looks like this:</p>
</div>


<div class="in">
<pre>num = 37
if num &gt; 100:
    print &#39;greater&#39;
else:
    print &#39;not greater&#39;
print &#39;done&#39;</pre>
</div>

<div class="out">
<pre>not greater
done
</pre>
</div>


<div class="">
<p>The second line of this code uses the keyword <code>if</code> to tell Python that we want to make a choice. If the test that follows it is true, the body of the <code>if</code> (i.e., the lines indented underneath it) are executed. If the test is false, the body of the <code>else</code> is executed instead. Only one or the other is ever executed:</p>
</div>


<div class="">
<p><img src="img/python-flowchart-conditional.svg" alt="Executing a Conditional" /></p>
</div>


<div class="">
<p>Conditional statements don't have to include an <code>else</code>. If there isn't one, Python simply does nothing if the test is false:</p>
</div>


<div class="in">
<pre>num = 53
print &#39;before conditional...&#39;
if num &gt; 100:
    print &#39;53 is greater than 100&#39;
print &#39;...after conditional&#39;</pre>
</div>

<div class="out">
<pre>before conditional...
...after conditional
</pre>
</div>


<div class="">
<p>We can also chain several tests together using <code>elif</code>, which is short for &quot;else if&quot;. This makes it simple to write a function that returns the sign of a number:</p>
</div>


<div class="in">
<pre>def sign(num):
    if num &gt; 0:
        return 1
    elif num == 0:
        return 0
    else:
        return -1

print &#39;sign of -3:&#39;, sign(-3)</pre>
</div>

<div class="out">
<pre>sign of -3: -1
</pre>
</div>


<div class="">
<p>One important thing to notice the code above is that we use a double equals sign <code>==</code> to test for equality rather than a single equals sign because the latter is used to mean assignment. This convention was inherited from C, and while many other programming languages work the same way, it does take a bit of getting used to...</p>
<p>We can also combine tests using <code>and</code> and <code>or</code>. <code>and</code> is only true if both parts are true:</p>
</div>


<div class="in">
<pre>if (1 &gt; 0) and (-1 &gt; 0):
    print &#39;both parts are true&#39;
else:
    print &#39;one part is not true&#39;</pre>
</div>

<div class="out">
<pre>one part is not true
</pre>
</div>


<div class="">
<p>while <code>or</code> is true if either part is true:</p>
</div>


<div class="in">
<pre>if (1 &lt; 0) or (&#39;left&#39; &lt; &#39;right&#39;):
    print &#39;at least one test is true&#39;</pre>
</div>

<div class="out">
<pre>at least one test is true
</pre>
</div>

<div class="">
<p>In this case, &quot;either&quot; means &quot;either or both&quot;, not &quot;either one or the other but not both&quot;.</p>
</div>

<h3>Challenges</h3>

<div class="challenges">
<ol style="list-style-type: decimal">
<li><p><code>True</code> and <code>False</code> aren't the only values in Python that are true and false. In fact, <em>any</em> value can be used in an <code>if</code> or <code>elif</code>. After reading and running the code below, explain what the rule is for which values are considered true and which are considered false. (Note that if the body of a conditional is a single statement, we can write it on the same line as the <code>if</code>.)</p>
<pre class="sourceCode python"><span class="kw">if</span> <span class="st">&#39;&#39;</span>: <span class="kw">print</span> <span class="st">&#39;empty string is true&#39;</span></pre>
<pre class="sourceCode python"><span class="kw">if</span> <span class="st">&#39;word&#39;</span>: <span class="kw">print</span> <span class="st">&#39;word is true&#39;</span></pre>
<pre class="sourceCode python"><span class="kw">if</span> []: <span class="kw">print</span> <span class="st">&#39;empty list is true&#39;</span></pre>
<pre class="sourceCode python"><span class="kw">if</span> [<span class="dv">1</span>, <span class="dv">2</span>, <span class="dv">3</span>]: <span class="kw">print</span> <span class="st">&#39;non-empty list is true&#39;</span></pre>
<pre class="sourceCode python"><span class="kw">if</span> <span class="dv">0</span>: <span class="kw">print</span> <span class="st">&#39;zero is true&#39;</span></pre>
<pre class="sourceCode python"><span class="kw">if</span> <span class="dv">1</span>: <span class="kw">print</span> <span class="st">&#39;one is true&#39;</span></pre></li>
<li><p>Write a function called <code>near</code> that returns <code>True</code> if its first parameter is within 10% of its second and <code>False</code> otherwise. Compare your implementation with your partner's: do you return the same answer for all possible pairs of numbers?</p></li>
</ol>
</div>

<h3>Nesting</h3>


<div class="">
<p>Another thing to realize is that <code>if</code> statements can be combined with loops just as easily as they can be combined with functions. For example, if we want to sum the positive numbers in a list, we can write this:</p>
</div>


<div class="in">
<pre>numbers = [-5, 3, 2, -1, 9, 6]
total = 0
for n in numbers:
    if n &gt;= 0:
        total = total + n
print &#39;sum of positive values:&#39;, total</pre>
</div>

<div class="out">
<pre>sum of positive values: 20
</pre>
</div>


<div class="">
<p>We could equally well calculate the positive and negative sums in a single loop:</p>
</div>


<div class="in">
<pre>pos_total = 0
neg_total = 0
for n in numbers:
    if n &gt;= 0:
        pos_total = pos_total + n
    else:
        neg_total = neg_total + n
print &#39;negative and positive sums are:&#39;, neg_total, pos_total</pre>
</div>

<div class="out">
<pre>negative and positive sums are: -6 20
</pre>
</div>


<div class="">
<p>We can even put one loop inside another:</p>
</div>


<div class="in">
<pre>for consonant in &#39;bcd&#39;:
    for vowel in &#39;ae&#39;:
        print consonant + vowel</pre>
</div>

<div class="out">
<pre>ba
be
ca
ce
da
de
</pre>
</div>


<div class="">
<p>As the diagram below shows, the <a href="../../gloss.html#inner-loop">inner loop</a> runs from start to finish each time the <a href="../../gloss.html#outer-loop">outer loop</a> runs once:</p>
</div>


<div class="">
<p><img src="img/python-flowchart-nested-loops.svg" alt="Execution of Nested Loops" /></p>
</div>



<div class="">
<p>This is our first hand-made data visualization: the colors show where <code>x</code> is less than, equal to, or greater than <code>y</code>.</p>
</div>

<h3>Challenges</h3>
<div class="challenges">

<ol style="list-style-type: decimal">
<li><p>Python (and most other languages in the C family) provides <a href="../../gloss.html#in-place-operator">in-place operators</a> that work like this:</p>
<pre class="sourceCode python"><code class="sourceCode python">x = <span class="dv">1</span>  <span class="co"># original value</span>
x += <span class="dv">1</span> <span class="co"># add one to x, assigning result back to x</span>
x *= <span class="dv">3</span> <span class="co"># multiply x by 3</span>
<span class="kw">print</span> x
<span class="dv">6</span></code></pre>
<p>Rewrite the code that sums the positive and negative numbers in a list using in-place operators. Do you think the result is more or less readable than the original?</p></li>
</ol>
</div>

<h3>Making Choices Analyzing Inflammation</h3>


<div class="">
<p>Let's say that we want to split up our original inflmmation dataset into individuals who had above average inflammation and those with below average inflammation over the entire time course.</p>
</div>

<p>First, let's make sure that our data is loaded:</p>
<div class="in">
<pre>data = numpy.loadtxt(fname=&#39;inflammation-01.csv&#39;, delimiter=&#39;,&#39;)</pre>
</div>

<p>Next, need to know what is above average and below average inflammation for an individual. For this, we can think back to one of our first exercises where we calculated the average inflammation:</p>

<div class="in">
<pre>print data.mean()</pre>
</div>

<p>and save that value to a variable:</p>
<div class="in">
<pre>ave_inflammation = data.mean()</pre>
</div>

<p>Now, let's use a loop to look at how individuals with high or low inflammation scores improved over time:</p>
<div class="in">
<pre>
for indv in range(0,data.shape[0]):
    if data[indv,:].mean() < ave_inflammation:
        first_ten = data[indv,0:10].mean()
        last_ten = data[indv,data.shape[1]-10:data.shape[1]].mean()
        print last_ten - first_ten
</pre>
</div>

<p>Note: numpy arrays have specific built-in functions for looping through elements in the array, however, this is a little easier to relate back to what we have talked about today.</p>

<h3>Challenges</h3>

<div class="challenges">
<ol style="list-style-type: decimal">
<li>Write a function called <code>ave_change(data, cutoff, direction=">")</code> that calculates the average change in inflammation over the first ten days to the last ten days for individuals with average inflammation above the average inflammation of the entire cohort (<code>cutoff</code>). Allow for the option to calculate the change in inflammation for individuals with average inflammation below the cutoff (<code>direction</code>). Use the outlined function below as a guide. As always, remember to document your code!</li>
</ol>
<div class="in">
<pre>
def ave_change(data, cutoff, direction=">"):
	if direction == ">":
		# if want to calculate improvement for individuals above cutoff
	else:
		# if want to calculate improvement for indvidiuals below cutoff		
</pre>
</div>

</div>


