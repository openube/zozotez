<p>Welcome to Zozotez. Zozotez, which means Lisp in French (infinitive zozoter, 
but we use <a href="http://en.wikipedia.org/wiki/T-V_distinction" rel="nofollow">polite form</a>) 
is a <strong>tail recursive</strong> Lisp interpreter which runs under any <tt>BrainFuck</tt> environment. 
It&#x27;s a fully LISP1 compliant interpreter if used with the largest bootstrap expression when started up. 
Without it it&#x27;s still LISP1, but with other symbols. See below. </p>

<p><img src="http://sylwester.no/gcodeimg/zozotez.png" /> </p>

<p>The summer 2010 and 2011 I created <a href="http://code.google.com/p/ebf-compiler/" rel="nofollow">EBF</a>. 
A Compiler for a superset of <tt>BrainFuck</tt> which makes <tt>BrainFuck</tt> object code. 
I wrote it because I wanted to learn how programming languages was bootstrapped. It is of course written in itself. 
EBF is the language I have written Zozotez. Zozotez is in itself extendable since it has the power of both functions 
and macroes :)  </p><p><tt>BrainFuck</tt> is an Esoteric language/turing tarpit that only has enough instructions 
to become Turing Complete which means that   it is possible to create any application with it, but not as easy as 
some other programming languages. Daniel Cristofani wrote so elegantly:  &quot;- a language designed for the amusement 
of programmers&quot;. For more information on <tt>Brainfuck</tt> see 
<a href="http://en.wikipedia.org/wiki/Brainfuck" rel="nofollow">Wikipedia</a> </p><p>The idea of creating Lisp in 
<tt>Brainfuck</tt> started in 2007 when talking with a collague with my new found fasination and time waster: To create 
small brainfuck programs. Later that  day we talked about favorite programming language and he&#x27;s was Lisp. 
He said that the language is so easy to implement that it can be written in <tt>BrainFuck</tt>. The original LISP was 
based on research done by AI researcher John <tt>McCarthy</tt> in the late 1950&#x27;s which produced a paper in 1958 
describing the language which also included a interpreter written in itself using only <tt>10*</tt> primitive operators 
that need to be implemented in some underlying machinecode. For a breif article on this see 
<a href="http://www.paulgraham.com/rootsoflisp.html" rel="nofollow">This article</a> which is slightly easier to understand 
than <tt>McCarthy&#x27;s</tt>  own paper which is more mathematical than programmer-oriented. </p><p><tt>*</tt> 
Actually it was 7 defined: quote,atom,eq,car,cdr,cons and cond. I say 10 because I have added list-lambda (functions) 
in addition to read and print for side effects. <tt>BrainFuck</tt> is actually Turing complete without the side effects too, 
but application would be harder to use as this must be emulated in either code or environment. </p><h2><a name="Why_do_this?"></a>Why do this?<a href="#Why_do_this?" class="section_anchor"></a></h2><p>Mostly for kicks. EBF and Zozotez are both projects that have everlasting enhancement potential while any puzzle is finished when you have laid the last piece. The fact that some people have thought about this, some has started, but never succeeded has motivated me as I feel I&#x27;m the first to successfully climb this mountain. I feel I have the know how to successfully implement LISP in any imperative machine architecture and not just LISP which relies on the underlying implementation to do certain things. <a href="http://michaux.ca/articles/scheme-from-scratch-bootstrap-v0_1-integers" rel="nofollow">Peter Michaux&#x27; Scheme implementation</a> has similar properties and is an excellent blog how he did it step by step.  </p><h1><a name="Syntax_overview"></a>Syntax overview<a href="#Syntax_overview" class="section_anchor"></a></h1><p>Zozotez is a LISP dialect, but because of it&#x27;s nature all except the symbols T and NIL have been given different, one char symbols. Here is the formal syntax with usual LISP equivalents. </p><h2><a name="Special_forms"></a>Special forms<a href="#Special_forms" class="section_anchor"></a></h2><ul><li><strong><tt>&quot;</tt></strong> (lisp quote) A quote returns the argument unevaluated. If you&#x27;d like to have expr unaltererd. eg. (<tt>&quot;</tt> expr) =&gt; expr </li><li><strong><tt>?</tt></strong> (lisp if) if evaluates only the first argument and if that is true it will evaluate the next argument. If it is false and it will evaluate the third argument.eg. (? T &#x27;one two) =&gt; one </li><li><strong><tt>\</tt></strong> (lisp lambda) whole expression returns unaltered </li><li><strong><tt>~</tt></strong> (lisp flambda) as lambda it returns unaltered </li></ul><h2><a name="Functions"></a>Functions<a href="#Functions" class="section_anchor"></a></h2><p>All functions evaluate all its arguments, even excess arguments, befor invoking the functions. <ul><li><strong><tt>=</tt></strong> (lisp eq) returns T if first argument is pointer equal to second or the same symbol. eg. (= &#x27;a &#x27;a) =&gt; T </li><li><strong><tt>s</tt></strong> (lisp atom) returns T if first argument is a symbol. eg (s NIL) =&gt; T </li><li><strong><tt>a</tt></strong> (lisp car) returns the first element of the first argument, which must be a list. eg. (a &#x27;(q w)) =&gt; q </li><li><strong><tt>d</tt></strong> (lisp cdr) returns the rest of the first argument, which must be a list. eg. (d &#x27;(q w)) =&gt; (w) </li><li><strong><tt>c</tt></strong> (lisp cons) returns a new list consisting of the first argument begin the first element and the second argument being the rest of the list. eg. (c &#x27;q &#x27;(w)) =&gt; (q w) </li><li><strong><tt>e</tt></strong> (lisp eval) evaluates the first argument. Note that, as a function, the argument has already been evaluated once. eg. (e &#x27;&#x27;q) =&gt; q </li><li><strong><tt>r</tt></strong> (lisp read) returns an unevaluated expression from keyboard. read can be used to read a line and will enclose that in a list. If what read is one expression it will return that. Excess closing parenthesis will be ignored but too few and it will still wait for more input. </li><li><strong><tt>p</tt></strong> (lisp print) returns the first argument unchanged. It also prints the first argument as an expression. If there is a second argument it will for symbols NOT terminate it with a newline and for a list it will omitt the outer parenthesis. eg. (p &#x27;(this is a test) T) =&gt; (this is a test) printed: this is a test\n </li><li><strong><tt>:</tt></strong> (lisp set) creates an asociation between the symbol in first argument to the expression in the second argument. eg. (= (: &#x27;test &#x27;(this is a test)) test) =&gt; T </li><li><strong><tt>(\)</tt></strong> (lisp (lambda)) returns the result from applying the user defined function definition </li><li><strong><tt>(~)</tt></strong> (lisp (lambda)) evaluates the return from the result from applying the user defined  macro definition </li></ul></p><h2><a name="User_defined_functions"></a>User defined functions<a href="#User_defined_functions" class="section_anchor"></a></h2><p>If an expression in the operation position evaluates to a list with the first element \ it is a lambda-expression. A lambda expression is a user defined function. One can define a new function with set in this manner: </p><pre class="prettyprint">(: &#x27;cons (\ (arg1 arg2) (c arg1 arg2)))
so that 
(cons &#x27;a &#x27;(b)) =&gt; (a b)</pre>

<p>We can also invoke it directly: </p>

<pre class="prettyprint">((\ (arg1 arg2) (c arg1 arg2)) &#x27;a &#x27;(b)) =&gt; (a b)</pre>

<p>Only the last expression returns something in a user defined function. 
If there are more than one expression it has to be for the side effects. </p>
<h2><a name="User_defined_macroes"></a>User defined macroes<a href="#User_defined_macroes" class="section_anchor"></a></h2>

<p>A macro is a function where the arguments are not evaluated before the execution and the resulting expression 
gets evaluated in the end. Thus: ((~(sym arg)(c :(c(c &quot;(c sym))(c arg))))) a &#x27;(b)) evaluates the return 
of the body, which is (: (&quot; a) (&quot; (b))) which assosiates a with (b). You might have noticed that this 
implements setq. </p>

<h2><a name="Implementation_limitation"></a>Implementation limitation<a href="#Implementation_limitation" class="section_anchor"></a></h2>

<p>When read reads a symbol it creates a hash using a similar method as EBF. It has one symbol table for both functions, 
macroes and variables and they are prone to collisions. In EBF collisions were errors requiring you to change the name 
to something else, while collisions in Zozotez are not handeled so <tt> (eq &#x27;p &#x27;ok) </tt> returns T since they 
both have the same hash. To check your symbols do <tt>&#x27;&lt;symbol&gt;</tt> in a REPL and it will echo the stored 
string (which will not be the same as you entered if it&#x27;s an collision). Together with dynamic scoping it is a serious 
flaw in Zozotez which I may fix in the future, but I feel there are other areas more cool (like garbage collection, 
numbers, tail call optimizations and precomputing (compilation) of functions. I also want to make lexical scoping, but 
the design with the o(1) hash lookup does not support such a scheme at this time. </p>

<h3><a name="Examples"></a>Examples<a href="#Examples" class="section_anchor"></a></h3>
<h4><a name="quine"></a>quine<a href="#quine" class="section_anchor"></a></h4>

<pre class="prettyprint">((\ (x) (list x (list (&quot; &quot;) x))) (&quot; (\ (x) (list x (list (&quot; &quot;) x)))))</pre>

<p>To browse visit <a href="http://ss.webring.com/navbar?f=l;y=webringcom44;u=defurl1" rel="nofollow">The Esoteric Programming Languages Ring</a> <br></br><br></br> <g:plusone size="medium" source="google:projecthosting"></g:plusone> </p>
