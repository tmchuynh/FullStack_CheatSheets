# Typescript

<p>One of the most interesting languages for large-scale application development is Microsoft’s <a href="https://typescriptlang.org" target="_blank" rel="noreferrer noopener nofollow">TypeScript</a>. TypeScript is unique in that it is a superset of JavaScript, but with optional types, interfaces, generics, and more. Unlike other compile-to-JavaScript languages, TypeScript does not try to change JavaScript into a new language. Instead, the TypeScript team is careful to align the language’s extra features as closely as possible with what’s available in JavaScript, both current and draft features. Because of this, TypeScript developers are able to take advantage of the latest features in the JavaScript language in addition to a powerful type system to write better-organized code, all while taking advantage of the advanced tooling that using a statically typed language can provide.</p>



<p>Tooling support is where TypeScript really shines. Modular code and static types allow for better-structured projects that are easier to maintain. This is especially important as JavaScript projects grow in size (both in terms of lines of code and developers on the project). Having fast, accurate completion, refactoring capabilities, and immediate feedback make TypeScript the ideal language for large-scale JavaScript.</p>



<p>Getting started with TypeScript is easy! Since vanilla JavaScript is effectively TypeScript without type annotations, much or all of an existing project can be used immediately and then updated over time to take advantage of all that TypeScript has to offer.</p>



<p>While TypeScript’s documentation has improved significantly since this guide was first posted, this Definitive Guide still provides one of the best overviews of the key features of TypeScript, assuming you already have a reasonable knowledge of JavaScript. The guide is regularly updated to provide new information about the latest versions of TypeScript.</p>



<h2>Installation and usage</h2>



<p>Installing TypeScript is as simple as running <code>npm install typescript</code>. Once installed, the TypeScript compiler is available by running <code>npx tsc</code>. If you want to try out TypeScript in your browser, the <a href="https://www.typescriptlang.org/play" target="_blank" rel="noreferrer noopener nofollow">TypeScript Playground</a> lets you experience TypeScript with a full code editor, with the limitation that modules cannot be used. Most of the examples in this guide can be pasted directly into the playground to quickly see how TypeScript compiles into easy-to-read JavaScript.</p>



<p>From the command line, the compiler can run in a couple of different modes, selectable with <a href="https://www.typescriptlang.org/docs/handbook/compiler-options.html#compiler-options">compiler options</a>. Just calling the executable will build the current project. Calling with <code>--noEmit</code> will type check the project but won’t emit any code. Adding a <code>--watch</code> option will start a server process that will continually watch a project and incrementally rebuild it whenever a file is changed, which can be much faster than performing a full compile from scratch. An <code>--incremental</code> flag was added in TS 3.4 that lets the compiler save some compiler states to a file, making subsequent full compiles faster (although not as fast as a watch-based rebuild).</p>



<h2>Configuration</h2>



<p>The TypeScript compiler is highly configurable, allowing the user to define where source files are located, how they should be transpiled, whether standard JavaScript files should be processed, and how strict the type checker should be. A <a href="https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#using-tsconfigjson" target="_blank" rel="noreferrer noopener nofollow">tsconfig.json</a> file identifies a project to the TypeScript compiler and contains settings used to build a TS project such as <a href="https://www.typescriptlang.org/docs/handbook/compiler-options.html" target="_blank" rel="noreferrer noopener nofollow">compiler flags</a>. Most of the configuration options can also be passed directly to the <code>tsc</code> command. This is the <code>tsconfig.json</code> from the <a href="https://dojo.io/" target="_blank" rel="noreferrer noopener">Dojo</a> project’s <a href="https://github.com/dojo/framework" target="_blank" rel="noreferrer noopener">framework</a> package:</p>



<pre class="wp-block-prismatic-blocks language-javascript" tabindex="0"><code class="language-javascript"><span class="token punctuation">{</span>
  <span class="token string-property property">"extends"</span><span class="token operator">:</span> <span class="token string">"./node_modules/@dojo/scripts/tsconfig/umd.json"</span><span class="token punctuation">,</span>
  <span class="token string-property property">"compilerOptions"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
    <span class="token string-property property">"jsx"</span><span class="token operator">:</span> <span class="token string">"react"</span><span class="token punctuation">,</span>
    <span class="token string-property property">"jsxFactory"</span><span class="token operator">:</span> <span class="token string">"tsx"</span><span class="token punctuation">,</span>
    <span class="token string-property property">"types"</span><span class="token operator">:</span> <span class="token punctuation">[</span> <span class="token string">"intern"</span> <span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token string-property property">"lib"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
      <span class="token string">"dom"</span><span class="token punctuation">,</span>
      <span class="token string">"es5"</span><span class="token punctuation">,</span>
      <span class="token string">"es2015.core"</span><span class="token punctuation">,</span>
      <span class="token string">"es2015.iterable"</span><span class="token punctuation">,</span>
      <span class="token string">"es2015.promise"</span><span class="token punctuation">,</span>
      <span class="token string">"es2015.symbol"</span><span class="token punctuation">,</span>
      <span class="token string">"es2015.symbol.wellknown"</span><span class="token punctuation">,</span>
      <span class="token string">"es2015.proxy"</span>
    <span class="token punctuation">]</span>
  <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token string-property property">"include"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
    <span class="token string">"./src/**/*.ts"</span><span class="token punctuation">,</span>
    <span class="token string">"./src/**/*.tsx"</span><span class="token punctuation">,</span>
    <span class="token string">"./tests/**/*.ts"</span><span class="token punctuation">,</span>
    <span class="token string">"./tests/**/*.tsx"</span>
  <span class="token punctuation">]</span>
<span class="token punctuation">}</span></code></pre>



<p>The <a href="https://www.typescriptlang.org/tsconfig#extends" target="_blank" rel="noreferrer noopener nofollow">extends</a> property indicates that this file is extending another <code>tsconfig.json</code> file; much like extending a class, the settings in the file being extended are used as defaults, and the settings in the file doing the extending are overrides. TypeScript 5.0 allows you to extend multiple settings files (specified as an array), with latter entries overriding earlier ones. This provides additional flexibility in structuring larger projects that may require different settings for independent areas while still maintaining a common config root.</p>



<p>The <a href="https://www.typescriptlang.org/tsconfig#jsx" target="_blank" rel="noreferrer noopener nofollow">jsx</a> property indicates that the project may use <a href="https://facebook.github.io/jsx/" target="_blank" rel="noreferrer noopener nofollow">JSX</a> syntax and that JSX should be transformed into React-style JavaScript. The <a href="https://www.typescriptlang.org/tsconfig#include" target="_blank" rel="noreferrer noopener nofollow">include</a> option tells the compiler which files to include in the compilation.</p>



<p>TypeScript provides <a href="https://www.typescriptlang.org/tsconfig" target="_blank" rel="noreferrer noopener nofollow">many options</a> to control how the compiler works, such as the ability to relax type checking strictness or to allow vanilla JavaScript files to be processed. This is one of the best parts of TypeScript: it allows TypeScript to be added to an existing project without requiring that the entire project be converted to fully-typed TypeScript. For example, the <a href="https://www.typescriptlang.org/tsconfig#noImplicitAny" target="_blank" rel="noreferrer noopener nofollow">noImplicitAny</a> flag, when <code>false</code>, will prevent the compiler from emitting warnings about untyped variables. Over time, a project can disable this and enable stricter processing options, allowing a team to work up, incrementally, towards fully-typed code. For new TypeScript projects, it is recommended that the <a href="https://www.typescriptlang.org/tsconfig#strict" target="_blank" rel="noreferrer noopener nofollow">strict</a> flag be enabled from the beginning to receive the full benefit of TypeScript.</p>



<p>As TypeScript has now been around for over a decade, some configuration options are no longer relevant in modern projects given the current state of language targets, runtimes, and tooling. TypeScript 5.0 deprecates some <a href="https://github.com/microsoft/TypeScript/issues/51000" target="_blank" rel="noreferrer noopener">legacy tsconfig settings</a> and projects still using them will have warnings issued when running <code>tsc</code>. Projects are advised to migrate away from these during the TS 5.x release cycle as they will eventually throw errors in a future TypeScript release (likely 6.0).</p>



<h2>Syntax and JavaScript support</h2>



<p>TypeScript supports current JavaScript syntax (through ES2022), as well as a number of draft language proposals. In <em>most</em> cases, TypeScript can emit code that’s compatible with older JavaScript runtimes even when using new features, allowing developers to write code using modern JS features that can still run in legacy environments.</p>



<p>Proposed JavaScript features supported by TypeScript include:</p>



<ul>
<li><a href="https://tc39.github.io/proposal-async-iteration/" target="_blank" rel="noreferrer noopener">for-await-of loop iteration</a> <em>(TS 2.3, ES3/ES5 with </em><a href="https://www.typescriptlang.org/tsconfig#downlevelIteration" target="_blank" rel="noreferrer noopener"><em>downlevelIteration</em></a><em>)</em></li>



<li><a href="https://tc39.github.io/proposal-dynamic-import/" target="_blank" rel="noreferrer noopener">Dynamic import expressions</a> <em>(TS 2.4)</em></li>



<li><a href="https://tc39.github.io/proposal-optional-catch-binding/" target="_blank" rel="noreferrer noopener">Optional catch clause variables</a> <em>(TS 2.5)</em></li>



<li><a href="https://tc39.es/proposal-import-meta/" target="_blank" rel="noreferrer noopener">import.meta</a> <em>(TS 2.9)</em></li>



<li><a href="https://tc39.es/proposal-optional-chaining/" target="_blank" rel="noreferrer noopener">Optional chaining</a> <em>(TS 3.7)</em></li>



<li><a href="https://tc39.es/proposal-nullish-coalescing/" target="_blank" rel="noreferrer noopener">Nullish coalescing</a> <em>(TS 3.7</em>)</li>



<li><a href="https://www.typescriptlang.org/docs/handbook/classes.html#ecmascript-private-fields" target="_blank" rel="noreferrer noopener">Private class fields</a> <em>(TS 3.8, ES2020, only supported for ES2015+ targets)</em></li>



<li><a href="https://tc39.es/proposal-export-ns-from/" target="_blank" rel="noreferrer noopener">export * as ns syntax</a> <em>(TS 3.8, ES2020)</em></li>



<li><a href="https://tc39.es/proposal-import-assertions/" target="_blank" rel="noreferrer noopener">import assertions</a> <em>(TS 4.5)</em></li>



<li><a href="https://github.com/tc39/proposal-decorators" target="_blank" rel="noreferrer noopener">TC39 Decorators</a><em> (TS 5.0, although </em><a href="https://www.typescriptlang.org/tsconfig#experimentalDecorators" target="_blank" rel="noreferrer noopener"><em>experimental</em></a><em> pre-TC39 decorators have been supported for several versions)</em></li>
</ul>



<p>There’s more to JavaScript than just syntax, though. TS also needs to understand the types used by the JavaScript standard library, which has changed over time. By default, the TS compiler emits ES5 code and assumes an ES5-compatible standard library. Note that the default target since TypeScript’s inception and prior to TS 5.0 was ES3, however, this target is now deprecated and teams still using it will encounter compiler warnings prompting them to upgrade to newer targets. So, for example, arrays won’t have an <code>includes</code> method. Setting the <a href="https://www.typescriptlang.org/tsconfig#target" target="_blank" rel="noreferrer noopener">target</a> compiler option to “es2016” (or “ES2016”; most TS option values aren’t case-sensitive) will instruct the compiler to emit ES2016 code, and also causes it to load several built-in ES2016 type libraries, one of which includes typings for <code>Array.prototype.includes</code>.</p>



<p>Note that these are <em>type</em> libraries, not polyfills. They tell the compiler what features arrays will have in the target environment, but do not actually provide any functionality themselves.</p>



<p>The <a href="https://www.typescriptlang.org/tsconfig#lib" target="_blank" rel="noreferrer noopener nofollow">lib</a> config property allows specific type subsets to be enabled to tailor the compiler’s output for a particular environment. For example, if a project will be running in a legacy environment that’s known to have a polyfill for&nbsp; <code>Array.prototype.includes</code>, then “<code>es2016.array.include</code>” could be added to the <code>lib</code> property to let the compiler know that this method (but not other ES2016 library methods) will be available. If code will be running in a browser, then “dom” should be added to <code>lib</code> to tell the compiler that global DOM resources will be available.</p>



<p>Different support libraries may also be used in specific files rather than the entire project with another TypeScript feature: <a href="https://www.typescriptlang.org/docs/handbook/triple-slash-directives.html#-reference-lib-" target="_blank" rel="noreferrer noopener nofollow">triple-slash directives</a>. These are single-line comments containing XML tags that specify compiler directives, such as the <code>lib</code> setting. For example:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token comment">/// &lt;reference lib="es2016.array.include" /&gt;</span>

<span class="token punctuation">[</span> <span class="token string">'foo'</span><span class="token punctuation">,</span> <span class="token string">'bar'</span><span class="token punctuation">,</span> <span class="token string">'baz'</span> <span class="token punctuation">]</span><span class="token punctuation">.</span><span class="token function">includes</span><span class="token punctuation">(</span><span class="token string">'bar'</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// true</span></code></pre>



<p>The compiler will not throw an error about the use of <code>Array.prototype.includes</code> in the module containing the directive. However, if another file in the project tried to use <code>includes</code>, the compiler would throw an error. Note that not all compiler directives can be provided with triple-slash directives, and also that these directives are only valid at the top of a TS file.</p>



<p>While TypeScript supports standard JavaScript syntax, it also adds some new syntax, such as type annotations, access modifiers (public, private), and support for generics. TS 3.4 added support for a new <a href="https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions" target="_blank" rel="noreferrer noopener nofollow">const assertion</a> that can be used to declare values as deeply constant (unlike JavaScript’s <code>const</code>, which only declares a variable itself as unwritable). These differences are additive; they don’t replace normal JS syntax, but add new capabilities in a syntax-compatible fashion.</p>



<h3>Imports and module resolution</h3>



<p>TypeScript files use the <code>.ts</code> file extension, and each file typically represents a module, similar to <a href="https://www.sitepen.com/blog/amd-the-definitive-source/" target="_blank" rel="noreferrer noopener nofollow">AMD</a>, <a href="http://wiki.commonjs.org/wiki/Modules/1.1" target="_blank" rel="noreferrer noopener nofollow">CommonJS</a>, and native <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules" target="_blank" rel="noreferrer noopener nofollow">JavaScript modules</a> (ESM) files. TypeScript uses a relaxed version of the JavaScript import API to import and export resources from modules:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">import</span> myModule <span class="token keyword">from</span> <span class="token string">'./myModule'</span><span class="token punctuation">;</span></code></pre>



<p>The main difference from standard ESM imports is that TypeScript doesn’t require absolute URLs and file extensions when referencing modules. It will assume a <code>.ts</code> or <code>.js</code> file extension, and uses a couple of different module resolution strategies to locate modules.</p>



<p>For AMD, SystemJS, and ES2015 modules, TypeScript defaults to its “classic” strategy, however modern projects will rarely encounter this. For any other module type, it defaults to its “node” strategy. The strategy can be manually set with the <a href="https://www.typescriptlang.org/tsconfig#moduleResolution" target="_blank" rel="noreferrer noopener nofollow">moduleResolution</a> config option.</p>



<p>TypeScript 5.0 introduces a new module resolution strategy of <code>bundler</code>. As its name suggests, this strategy aims to replicate the more flexible resolution rules of modern application toolchains that rely on bundlers or TypeScript-native runtimes. It is closer to the existing <code>node</code> strategy without restrictions introduced by the <code>node16</code>/<code>nodenext</code> strategies that TypeScript 4.7 brought in, such as requiring file extensions in relative imports.</p>



<p>The <code>node</code> strategy emulates Node’s module resolution logic. Relative module IDs are resolved relative to the directory containing the referencing module and will consider the “main” field in a <code>package.json</code> if present. Absolute module IDs are resolved by first looking for the referenced module in a local <code>node_modules</code> directory, and then by walking up the directory hierarchy, looking for the module in <code>node_modules</code> directories.</p>



<p>When the <code>classic</code> strategy is in use, relative module IDs are resolved relative to the directory containing the referencing module. For absolute module IDs, the compiler walks up the filesystem, starting from the directory containing the referencing module, looking for <code>.ts</code>, then <code>.d.ts</code>, in each parent directory, until it finds a match.</p>



<p>The <a href="https://www.typescriptlang.org/tsconfig#baseUrl" target="_blank" rel="noreferrer noopener nofollow">baseUrl</a>, <a href="https://www.typescriptlang.org/tsconfig#paths" target="_blank" rel="noreferrer noopener nofollow">paths</a>, and <a href="https://www.typescriptlang.org/tsconfig#rootDirs" target="_blank" rel="noreferrer noopener nofollow">rootDirs</a> options can be used to further configure where the compiler looks for absolutely-referenced modules. Starting in TypeScript 4.7, you can use the <a href="https://www.typescriptlang.org/tsconfig#moduleSuffixes" target="_blank" rel="noreferrer noopener">moduleSuffixes</a> compiler option to give the compiler additional file name suffixes to use when resolving modules.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token punctuation">{</span>
 <span class="token string-property property">"compilerOptions"</span><span class="token operator">:</span> <span class="token punctuation">{</span>
   <span class="token string-property property">"moduleSuffixes"</span><span class="token operator">:</span> <span class="token punctuation">[</span><span class="token string">".ios"</span><span class="token punctuation">,</span> <span class="token string">".native"</span><span class="token punctuation">,</span> <span class="token string">""</span><span class="token punctuation">]</span>
 <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
<span class="token keyword">import</span> MyClass <span class="token keyword">from</span> <span class="token string">"./myModule"</span><span class="token punctuation">;</span></code></pre>



<p>Given the compiler options setting above, the import statement would cause the compiler to look for files named “myModule.ios.ts”, “myModule.native.ts”, and “myModule.ts”, stopping with the first one found.</p>



<p><a href="https://devblogs.microsoft.com/typescript/announcing-typescript-5-0/#resolution-customization-flags" target="_blank" rel="noreferrer noopener">TypeScript 5.0 introduces several more compiler options</a> to allow even finer-grained resolution customization if needed: <code>allowImportingTsExtensions</code>, <code>resolvePackageJsonExports</code>, <code>resolvePackageJsonImports</code>, <code>allowArbitraryExtensions</code>, and <code>customConditions</code>.</p>



<h2>Basic types</h2>



<p>Types are the banner feature of TypeScript. The TS compiler determines a type for every value (variable, function argument, return value, etc.) in a program, and it uses these types for a range of features, from indicating when a function is being called with the wrong input to enabling an IDE to auto-complete a class property name.</p>



<p>Without additional type hints, all variables in TypeScript have the <a href="https://www.typescriptlang.org/docs/handbook/basic-types.html#any" target="_blank" rel="noreferrer noopener nofollow">any</a> type, meaning they are allowed to contain any type of data, just like a JavaScript variable. The basic syntax for adding type constraints to code in TypeScript looks like this:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">toNumber</span><span class="token punctuation">(</span>numberString<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">number</span> <span class="token punctuation">{</span>
  <span class="token keyword">const</span> num<span class="token operator">:</span> <span class="token builtin">number</span> <span class="token operator">=</span> <span class="token function">parseFloat</span><span class="token punctuation">(</span>numberString<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">return</span> num<span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>The bolded type hints in the code above indicate that <code>toNumber</code> accepts one string parameter, and that it returns a number. The variable <code>num</code> is also explicitly typed to contain a number. Note that in many cases <strong>explicit type hints are not required</strong> (although it still may be beneficial to provide them) because TypeScript can <em>infer</em> them from the code itself. For example, the <code>number</code> type could be left off of the <code>num</code> declaration, because the TS compiler knows <code>parseFloat</code> returns a number. Similarly, the number return type isn’t required because the compiler knows that the function always returns a number.</p>



<p>The primitive types that TypeScript provides match the primitive types of JavaScript itself: <code>&lt;a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#any" target="_blank" rel="noreferrer noopener"&gt;any&lt;/a&gt;</code>, <a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean" target="_blank" rel="noreferrer noopener"><code>string</code>, <code>number</code>, <code>boolean</code></a>. TypeScript also has <a href="https://www.typescriptlang.org/docs/handbook/2/functions.html#void"><code>void</code></a> (for null or undefined function return values), <a href="https://www.typescriptlang.org/docs/handbook/2/narrowing.html#the-never-type"><code>never</code></a>, and as of TypeScript 3.0, <a href="https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-0.html#new-unknown-top-type"><code>unknown</code></a>.</p>



<p>In most cases, <code>never</code> is inferred for functions where the compiler detects unreachable code, so developers won’t often use <code>never</code> directly. For example, if a function only throws, it will have a return type of <code>never</code>.</p>



<p><code>unknown</code> is the type-safe counterpart of <code>any</code>; anything can be assigned to an unknown variable, but an unknown value can’t be assigned to anything other than an <code>any</code> variable without a type assertion or type narrowing.</p>



<p>When writing an expression (function call, arithmetic operation, etc.), you can also explicitly indicate the resulting type of the expression with a <a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions" target="_blank" rel="noreferrer noopener">type assertion</a>, which is necessary if you are calling a function where TypeScript cannot figure out the return type automatically. For example:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">numberStringSwap</span><span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token builtin">any</span><span class="token punctuation">,</span> radix<span class="token operator">:</span> <span class="token builtin">number</span> <span class="token operator">=</span> <span class="token number">10</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">any</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token keyword">typeof</span> value <span class="token operator">===</span> <span class="token string">'string'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token function">parseInt</span><span class="token punctuation">(</span>value<span class="token punctuation">,</span> radix<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token keyword">typeof</span> value <span class="token operator">===</span> <span class="token string">'number'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token function">String</span><span class="token punctuation">(</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>  
 
<span class="token keyword">const</span> num <span class="token operator">=</span> <span class="token function">numberStringSwap</span><span class="token punctuation">(</span><span class="token string">'1234'</span><span class="token punctuation">)</span> <span class="token keyword">as</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> str <span class="token operator">=</span> <span class="token operator">&lt;</span><span class="token builtin">string</span><span class="token operator">&gt;</span> <span class="token function">numberStringSwap</span><span class="token punctuation">(</span><span class="token number">1234</span><span class="token punctuation">)</span><span class="token punctuation">;</span></code></pre>



<p>In this example, the return value of <code>numberStringSwap</code> has been declared as <code>any</code> because the function might return more than one type. In order to remove the ambiguity, the type of the expression being assigned to <code>num</code> is explicitly asserted by the <code>as number</code> modifier after the call to <code>numberStringSwap</code>.</p>



<p>Type assertions must be made to compatible types. If TypeScript knew that <code>numberStringSwap</code> returned a string on line 10, attempting to assert that the value was a number would result in a compiler error (“Cannot convert string to number”) since the two types are known to be incompatible.</p>



<p>There is also a legacy syntax for type-casting that uses angle brackets (<code>&lt;&gt;</code>), as shown in line 11 above. The semantics for using angle brackets is the same as for using <code>as</code>. This used to be the default syntax, but it was replaced by <code>as</code> due to conflicts with JSX syntax (more on that later).</p>



<p>When writing code in TypeScript, it is a good practice to explicitly add types to your variables and functions when types cannot be inferred, or when you want to ensure a certain type (such as a function return type), or just for documentation. When a variable is not annotated and the type cannot be inferred, it is given an implicit <code>any</code> type. The <code>&lt;a href="https://www.typescriptlang.org/tsconfig#noImplicitAny" target="_blank" rel="noreferrer noopener"&gt;noImplicitAny&lt;/a&gt;</code> compiler option can be set in the <code>tsconfig.json</code> or on the command line and will prevent <code>any</code> accidental implicit any types from sneaking into your code.</p>



<h3>String Literal Types</h3>



<p>TypeScript also has support for <a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types" target="_blank" rel="noreferrer noopener nofollow">string literal types</a>. These are useful when you know that the value of a parameter can match one of a list of strings, for example:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> easing<span class="token operator">:</span> <span class="token string">"ease-in"</span> <span class="token operator">|</span> <span class="token string">"ease-out"</span> <span class="token operator">|</span> <span class="token string">"ease-in-out"</span><span class="token punctuation">;</span></code></pre>



<p>The compiler will check that any assignment to <code>easing</code> has one of the three values: <code>ease-in</code>, <code>ease-out</code>, or <code>ease-in-out</code>.</p>



<h3>Template literal types</h3>



<p>Template literal types, added in TypeScript 4.1, built upon the foundation established by string literal types. While string literal types must be represented by fixed strings, template literal types can derive their values using syntax that is very similar to template literals. Consider the scenario of describing the alignment of one element to another. To fully describe the possibilities, the horizontal and vertical dimensions must both be addressed. This might lead to the following types:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">type</span> <span class="token class-name">VerticalAlignment</span> <span class="token operator">=</span> <span class="token string">"top"</span> <span class="token operator">|</span> <span class="token string">"middle"</span> <span class="token operator">|</span> <span class="token string">"bottom"</span><span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">HorizontalAlignment</span> <span class="token operator">=</span> <span class="token string">"left"</span> <span class="token operator">|</span> <span class="token string">"center"</span> <span class="token operator">|</span> <span class="token string">"right"</span><span class="token punctuation">;</span></code></pre>



<p>Template literal types allow a function to be defined that can only accept a VerticalAlignment type concatenated to a HorizontalAlignment type with a dash separating the two values as shown here:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">declare</span> <span class="token keyword">function</span> <span class="token function">setAlignment</span><span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token template-string"><span class="token template-punctuation string">`</span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span>VerticalAlignment<span class="token interpolation-punctuation punctuation">}</span></span><span class="token string">-</span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span>HorizontalAlignment<span class="token interpolation-punctuation punctuation">}</span></span><span class="token template-punctuation string">`</span></span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token keyword">void</span></code></pre>



<p>The <code>setAlignment</code> function declared above will only accept valid strings without having to explicitly list each of the nine possible combinations of horizontal and vertical alignment.</p>



<h3>Object types</h3>



<p>In addition to the primitive types, TypeScript allows complex types (like objects and functions) to be easily defined and used in type constraints. Just as object literals are at the root of most object definitions in JavaScript, the <em>object type literal</em> is at the root of most object type definitions in TypeScript. In its most basic form, it looks very similar to a normal JavaScript object literal:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> point<span class="token operator">:</span> <span class="token punctuation">{</span>
  x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre>



<p>In this example, the <code>point</code> variable is defined as accepting any object with numeric <code>x</code> and <code>y</code> properties. Note that, unlike a normal object literal, the object type literal <strong>separates fields using semicolons</strong>, not commas.</p>



<p>TypeScript also includes an <code>object</code> type, which represents any non-primitive value (i.e., not a number, string, etc.). This type is distinct from <code>Object</code>, which can represent any JavaScript type (including primitives). For example, <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create" target="_blank" rel="noreferrer noopener nofollow">Object.create</a>‘s first argument must be an object (a non-primitive) or null. If this argument is typed as an <code>Object</code>, TypeScript will allow primitive values to be passed to <code>Object.create</code>, which would cause a runtime error. When the argument is typed as an <code>object</code>, TypeScript will only allow non-primitive values to be used. The <code>object</code> type is also distinct from object type literals since it doesn’t specify any structure for an object.</p>



<p>When TypeScript compares two different object types to decide whether or not they match, it does so <em>structurally</em>. This means that instead of checking whether two values both inherit from a shared ancestor type, as typing checking in many other languages does, the compiler instead compares the properties of each object to see if they are compatible. If an object being assigned has all of the properties that are required by the constraint on the variable being assigned to, and the property types are compatible, then the two types are considered compatible:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> point<span class="token operator">:</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token comment">// OK, properties match</span>
point <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token number">0</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token comment">// Error, x property type is wrong</span>
point <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token string">'zero'</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token number">0</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token comment">// Error, missing required property y</span>
point <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token number">0</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token comment">// Error, object literal may only specify known properties</span>
point <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> z<span class="token operator">:</span> <span class="token number">0</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token keyword">const</span> otherPoint <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> z<span class="token operator">:</span> <span class="token number">0</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token comment">// OK, extra properties not relevant for non-literal assignment</span>
point <span class="token operator">=</span> otherPoint<span class="token punctuation">;</span></code></pre>



<p>Note the error when assigning a literal object with an extra property.&nbsp; Literal values are checked more strictly than non-literals.</p>



<p>In order to reduce type duplication, the <code>typeof</code> operator can be used to reference the type of a value. For instance, if we were to add a <code>point2</code> variable, instead of having to write this:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> point<span class="token operator">:</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
<span class="token keyword">let</span> point2<span class="token operator">:</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> <span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre>



<p>We could instead simply reference the type of <code>point</code> using <code>typeof</code>:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> point<span class="token operator">:</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
<span class="token keyword">let</span> point2<span class="token operator">:</span> <span class="token keyword">typeof</span> point<span class="token punctuation">;</span></code></pre>



<p>This mechanism helps to reduce the amount of code we need to reference the same type, but there is another even more powerful abstraction in TypeScript for reusing object types: <em>interfaces</em>. An interface is, in essence, a <strong>named</strong> object type literal. Changing the previous example to use an interface would look like this:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span>
  x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">let</span> point<span class="token operator">:</span> Point<span class="token punctuation">;</span>
<span class="token keyword">let</span> point2<span class="token operator">:</span> Point<span class="token punctuation">;</span></code></pre>



<p>This change allows the <code>Point</code> type to be used in multiple places within the code without having to redefine the type’s details over and over again. Interfaces can also extend other interfaces or classes using the <code>extends</code> keyword in order to compose more complex types out of simple types:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Point3d</span> <span class="token keyword">extends</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span>
  z<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>In this example, the resulting <code>Point3d</code> type would consist of the <code>x</code> and <code>y</code> properties of the <code>Point</code> interface, plus the new <code>z</code> property.</p>



<p>Methods and properties on objects can also be specified as optional in the same way function parameters can be made optional:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span>
  x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  z<span class="token operator">?</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>Here, instead of specifying a separate interface for a three-dimensional point, we simply make the <code>z</code> property of the interface optional; the resulting type checking would look like this:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> point<span class="token operator">:</span> Point<span class="token punctuation">;</span>

<span class="token comment">// OK, properties match</span>
point <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> z<span class="token operator">:</span> <span class="token number">0</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token comment">// OK, properties match, optional property missing</span>
point <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token number">0</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token comment">// Error, `z` property type is wrong</span>
point <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token number">0</span><span class="token punctuation">,</span> z<span class="token operator">:</span> <span class="token string">'zero'</span> <span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre>



<p>So far, we’ve looked at object types with properties but haven’t specified how to add a <strong>method</strong> to an object. Because functions are first-class objects in JavaScript, they can be typed like any other object property (we’ll talk more about functions later):</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span>
  x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  z<span class="token operator">?</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>

  <span class="token function-variable function">toGeo</span><span class="token operator">:</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> Point<span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>Here we’ve declared a <code>toGeo</code> property on <code>Point</code> with the type <code>() =&gt; Point</code> (a function that takes no arguments and returns a <code>Point</code>). TypeScript also provides a shorthand syntax for specifying methods, which becomes very convenient later when we start working with <a href="https://www.sitepen.com/blog/advanced-typescript-concepts-classes-and-types" target="_blank" rel="noreferrer noopener">classes</a>:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span>
  x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  z<span class="token operator">?</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>

  <span class="token function">toGeo</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> Point<span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>Like properties, methods can also be made optional by putting a question mark after the method name:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span>
  <span class="token comment">// ...</span>
  toGeo<span class="token operator">?</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> Point<span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>By default, optional properties are treated as having a type like “[original type] | undefined”. So in the previous example, <code>toGeo</code> would be of type <code>Point | undefined</code>. This means that you can define a <code>Point</code> object like,</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">const</span> p<span class="token operator">:</span> Point <span class="token operator">=</span> <span class="token punctuation">{</span>
   toGeo<span class="token operator">:</span> <span class="token keyword">undefined</span>
<span class="token punctuation">}</span></code></pre>



<p>Normally this would be OK, but some built-in functions like <code>Object.assign</code> and <code>Object.keys</code> behave differently whether or not a property exists (and is undefined) or not.&nbsp; As of TypeScript 4.4, you can now use the option <code>exactOptionalPropertyTypes</code> to tell TypeScript to not allow undefined values in these cases.</p>



<p>Objects that are intended to be used as hash maps or ordered lists can be given an <a href="https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html" target="_blank" rel="noreferrer noopener">index signature</a>, which enables arbitrary keys to be defined on an object:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">HashMapOfPoints</span> <span class="token punctuation">{</span>
  <span class="token punctuation">[</span>key<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">]</span><span class="token operator">:</span> Point<span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>In this example, we’ve defined a type where arbitrary string keys can be set, so long as the assigned value is of type <code>Point</code>. Before TypeScript 4.4, as in JavaScript, it is only possible to use <code>string</code> or <code>number</code> as the type of the index signature. As of TypeScript 4.4 however, index signatures can also include Symbols and template string patterns.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">const</span> serviceUrl <span class="token operator">=</span> <span class="token function">Symbol</span><span class="token punctuation">(</span><span class="token string">"ServiceUrl"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> servicePort <span class="token operator">=</span> <span class="token function">Symbol</span><span class="token punctuation">(</span><span class="token string">"ServicePort"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
 
<span class="token keyword">interface</span> <span class="token class-name">Configuration</span> <span class="token punctuation">{</span>
   <span class="token punctuation">[</span>key<span class="token operator">:</span> <span class="token builtin">symbol</span><span class="token punctuation">]</span><span class="token operator">:</span> <span class="token builtin">string</span> <span class="token operator">|</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
   <span class="token punctuation">[</span>key<span class="token operator">:</span> <span class="token template-string"><span class="token template-punctuation string">`</span><span class="token string">service-</span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span><span class="token builtin">string</span><span class="token interpolation-punctuation punctuation">}</span></span><span class="token template-punctuation string">`</span></span><span class="token punctuation">]</span><span class="token operator">:</span> <span class="token builtin">string</span> <span class="token operator">|</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
 
<span class="token keyword">const</span> config<span class="token operator">:</span> Configuration <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token punctuation">}</span><span class="token punctuation">;</span>
 
config<span class="token punctuation">[</span>serviceUrl<span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token string">"my-url"</span><span class="token punctuation">;</span>
config<span class="token punctuation">[</span>servicePort<span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">8080</span><span class="token punctuation">;</span>
config<span class="token punctuation">[</span><span class="token string">"service-host"</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token string">"host"</span><span class="token punctuation">;</span>
config<span class="token punctuation">[</span><span class="token string">"service-port"</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">8080</span><span class="token punctuation">;</span>
config<span class="token punctuation">[</span><span class="token string">"host"</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token string">"host"</span><span class="token punctuation">;</span> <span class="token comment">// error</span></code></pre>



<p>For object types without an index signature, TypeScript will only allow properties to be set that are explicitly defined on the type. If you try to assign to a property that doesn’t exist on the type, you will get a compiler error. Occasionally, though, you <strong>do</strong> want to add dynamic properties to an object without an index signature. To do so, you can simply use array notation to set the property on the object: <code>a['foo'] = 'foo'</code>. Note, however, that using this workaround defeats the type system for these properties, so only do this as a last resort.</p>



<p>Interface properties can also be named using constant values, similar to computed property names on normal objects. Computed values must be constant strings, numbers, or Symbols:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">const</span> Foo <span class="token operator">=</span> <span class="token string">'Foo'</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> Bar <span class="token operator">=</span> <span class="token string">'Bar'</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> Baz <span class="token operator">=</span> <span class="token function">Symbol</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">interface</span> <span class="token class-name">MyInterface</span> <span class="token punctuation">{</span>
  <span class="token punctuation">[</span>Foo<span class="token punctuation">]</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  <span class="token punctuation">[</span>Bar<span class="token punctuation">]</span><span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
  <span class="token punctuation">[</span>Baz<span class="token punctuation">]</span><span class="token operator">:</span> <span class="token builtin">boolean</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<h3>Tuple types</h3>



<p>While JavaScript itself doesn’t have <a href="https://www.typescriptlang.org/docs/handbook/2/objects.html#tuple-types" target="_blank" rel="noreferrer noopener">tuples</a>, TypeScript makes it possible to emulate typed tuples using arrays. If you wanted to store a point as an <code>(x, y, z)</code> tuple instead of as an object, this can be done by specifying a tuple type on a variable:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> point<span class="token operator">:</span> <span class="token punctuation">[</span> <span class="token builtin">number</span><span class="token punctuation">,</span> <span class="token builtin">number</span><span class="token punctuation">,</span> <span class="token builtin">number</span> <span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">[</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">0</span> <span class="token punctuation">]</span><span class="token punctuation">;</span></code></pre>



<p>TypeScript 3.0 improved support for tuple types by allowing them to be used with rest and spread expressions, and by allowing for optional elements.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">draw</span><span class="token punctuation">(</span><span class="token operator">...</span>point<span class="token operator">:</span> <span class="token punctuation">[</span> <span class="token builtin">number</span><span class="token punctuation">,</span> <span class="token builtin">number</span><span class="token punctuation">,</span> <span class="token builtin">number</span><span class="token operator">?</span> <span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token keyword">void</span> <span class="token punctuation">{</span>
  <span class="token keyword">const</span> <span class="token punctuation">[</span> x<span class="token punctuation">,</span> y<span class="token punctuation">,</span> z <span class="token punctuation">]</span> <span class="token operator">=</span> point<span class="token punctuation">;</span>
  <span class="token builtin">console</span><span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'point'</span><span class="token punctuation">,</span> <span class="token operator">...</span>point<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token function">draw</span><span class="token punctuation">(</span><span class="token number">100</span><span class="token punctuation">,</span> <span class="token number">200</span><span class="token punctuation">)</span><span class="token punctuation">;</span>         <span class="token comment">// logs: point 100, 200</span>
<span class="token function">draw</span><span class="token punctuation">(</span><span class="token number">100</span><span class="token punctuation">,</span> <span class="token number">200</span><span class="token punctuation">,</span> <span class="token number">75</span><span class="token punctuation">)</span><span class="token punctuation">;</span>     <span class="token comment">// logs: point 100, 200, 75</span>
<span class="token function">draw</span><span class="token punctuation">(</span><span class="token number">100</span><span class="token punctuation">,</span> <span class="token number">200</span><span class="token punctuation">,</span> <span class="token number">75</span><span class="token punctuation">,</span> <span class="token number">25</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Error: Expected 2-3 arguments but got 4</span></code></pre>



<p>In the above example, the <code>draw</code> function can accept values for <code>x</code>, <code>y</code>, and optionally <code>z</code>. TypeScript 4.0 further enhanced tuple types by allowing for variable length tuple types and labeled tuple elements.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> point<span class="token operator">:</span> <span class="token punctuation">[</span>x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">,</span> z<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>
 
<span class="token keyword">function</span> <span class="token generic-function"><span class="token function">concat</span><span class="token generic class-name"><span class="token operator">&lt;</span><span class="token constant">T</span><span class="token punctuation">,</span> <span class="token constant">U</span><span class="token operator">&gt;</span></span></span><span class="token punctuation">(</span>arr1<span class="token operator">:</span> <span class="token constant">T</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">,</span> arr2<span class="token operator">:</span> <span class="token constant">U</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">Array</span><span class="token operator">&lt;</span><span class="token constant">T</span> <span class="token operator">|</span> <span class="token constant">U</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token punctuation">[</span><span class="token operator">...</span>arr1<span class="token punctuation">,</span> <span class="token operator">...</span>arr2<span class="token punctuation">]</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>The above example uses labeled tuples to make the <code>point</code> type more readable and shows an example of using variadic tuple types to write more concise types for functions that work on general tuple types.</p>



<p>TypeScript 4.2 improved tuples by allowing .<code>..rest</code> elements to be at any position in a tuple type.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> bar<span class="token operator">:</span> <span class="token punctuation">[</span><span class="token builtin">boolean</span><span class="token punctuation">,</span> <span class="token operator">...</span><span class="token builtin">string</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">,</span> <span class="token builtin">boolean</span><span class="token punctuation">]</span><span class="token punctuation">;</span></code></pre>



<p>There are restrictions.&nbsp; A rest element cannot be followed by another rest element or an optional element.&nbsp; There can only be one rest element in a tuple type.</p>



<h3>Function types</h3>



<p>Function types are typically defined using arrow syntax:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> <span class="token function-variable function">printPoint</span><span class="token operator">:</span> <span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token builtin">string</span><span class="token punctuation">;</span></code></pre>



<p>Here the variable <code>printPoint</code> is described as accepting a function that takes a Point argument and returns a string. The same syntax is used to describe a function argument to another function:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> <span class="token function-variable function">printPoint</span><span class="token operator">:</span> <span class="token punctuation">(</span><span class="token function-variable function">getPoint</span><span class="token operator">:</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> Point<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token builtin">string</span><span class="token punctuation">;</span></code></pre>



<p>Note the use of the arrow (<code>=&gt;</code>) to define the return type of the function. This differs from how the return type is written in a function declaration, where a colon (:) is used:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">printPoint</span><span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span>
<span class="token keyword">const</span> printPoint <span class="token operator">=</span> <span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span></code></pre>



<p>This can be a bit confusing at first, but as you work with TypeScript, you will find it is easy to know when one or the other should be used. For instance, in the original <code>printPoint</code> example, using a colon would <em>look</em> wrong because it would result in two colons directly within the constraint:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> printPoint<span class="token operator">:</span> <span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span></code></pre>



<p>Similarly, using an arrow with an arrow function would look wrong:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">const</span> <span class="token function-variable function">printPoint</span> <span class="token operator">=</span> <span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token builtin">string</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span></code></pre>



<p>Functions can also be described using the object literal syntax:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> printPoint<span class="token operator">:</span> <span class="token punctuation">{</span> <span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span> <span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre>



<p>This is effectively describing <code>printPoint</code> as a callable object (which is what a JavaScript function is).</p>



<p>Functions can be typed as constructors by putting the <code>new</code> keyword before the function type:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> Point<span class="token operator">:</span> <span class="token punctuation">{</span> <span class="token keyword">new</span> <span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> Point<span class="token punctuation">;</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
<span class="token keyword">let</span> Point<span class="token operator">:</span> <span class="token keyword">new</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> Point<span class="token punctuation">;</span></code></pre>



<p>In this example, any function assigned to <code>Point</code> would need to be a constructor that creates <code>Point</code> objects.</p>



<p>Because the object literal syntax allows us to define objects as functions, it’s also possible to define function types with static properties or methods (like the JavaScript <code>String</code> function, which also has a static method <code>String.fromCharCode</code>):</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> Point<span class="token operator">:</span> <span class="token punctuation">{</span>
  <span class="token keyword">new</span> <span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> Point<span class="token punctuation">;</span>
  <span class="token function">fromLinear</span><span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span><span class="token operator">:</span> Point<span class="token punctuation">;</span>
  <span class="token function">fromGeo</span><span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span><span class="token operator">:</span> Point<span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre>



<p>Here, we’ve defined <code>Point</code> as a constructor that also needs to have static <code>Point.fromLinear</code> and <code>Point.fromGeo</code> methods. The only way to actually do this is to define a <a href="https://www.sitepen.com/blog/advanced-typescript-concepts-classes-and-types" target="_blank" rel="noreferrer noopener">class</a> that implements <code>Point</code> and has static <code>fromLinear</code> and <code>fromGeo</code> methods; we’ll look at how to do this later when we discuss classes in depth.</p>



<p>As of TypeScript 3.1, static fields may also be added to functions simply by assigning to them:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">createPoint</span><span class="token punctuation">(</span>x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">Point</span><span class="token punctuation">(</span>x<span class="token punctuation">,</span> y<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

createPoint<span class="token punctuation">.</span><span class="token function">print</span><span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span> <span class="token punctuation">{</span>
  <span class="token comment">// print a point</span>
<span class="token punctuation">}</span>

Point p <span class="token operator">=</span> <span class="token function">createPoint</span><span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

createPoint<span class="token punctuation">.</span><span class="token function">print</span><span class="token punctuation">(</span>p<span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// prints the point</span></code></pre>



<h3>Overloaded functions</h3>



<p>Earlier, we created an example <code>numberStringSwap</code> function that converts between numbers and strings:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">numberStringSwap</span><span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token builtin">any</span><span class="token punctuation">,</span> radix<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">any</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token keyword">typeof</span> value <span class="token operator">===</span> <span class="token string">'string'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token function">parseInt</span><span class="token punctuation">(</span>value<span class="token punctuation">,</span> radix<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token keyword">typeof</span> value <span class="token operator">===</span> <span class="token string">'number'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token function">String</span><span class="token punctuation">(</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>We know that this function returns a string when it is passed a number, and a number when it is passed a string. However, the call signature doesn’t indicate this — since <code>any</code> is used for the value and return types, TypeScript doesn’t know what specific types of values are acceptable or what type will be returned. We can use <em>function overloads</em> to let the compiler know more about how the function actually works.</p>



<p>One way to write the above function, in which typing is correctly handled, is:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">numberStringSwap</span><span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">,</span> radix<span class="token operator">?</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
<span class="token keyword">function</span> <span class="token function">numberStringSwap</span><span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token keyword">function</span> <span class="token function">numberStringSwap</span><span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token builtin">any</span><span class="token punctuation">,</span> radix<span class="token operator">:</span> <span class="token builtin">number</span> <span class="token operator">=</span> <span class="token number">10</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">any</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token keyword">typeof</span> value <span class="token operator">===</span> <span class="token string">'string'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token function">parseInt</span><span class="token punctuation">(</span>value<span class="token punctuation">,</span> radix<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token keyword">typeof</span> value <span class="token operator">===</span> <span class="token string">'number'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token function">String</span><span class="token punctuation">(</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>With the above types, TypeScript now knows that the function can be called in two ways: with a number and optional radix, or with a string. If it’s called with a number, it will return a string, and vice versa. You can also use <a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types" target="_blank" rel="noreferrer noopener">union types</a> in some cases instead of function overloads, which will be discussed later in this guide.</p>



<p>It is extremely important to keep in mind that the concrete function implementation must have an interface that matches the lowest common denominator of all of the overload signatures. This means that if a parameter accepts multiple types, as <code>value</code> does here, the concrete implementation must specify a type that encompasses <strong>all</strong> the possible options. In the case of <code>numberStringSwap</code>, because <code>string</code> and <code>number</code> have no common base, the type for <code>value</code> <em>must</em> be <code>any</code> (or a union type).</p>



<p>Similarly, if different overloads accept different numbers of arguments, <strong>any arguments that do not exist in all overload signatures must be optional</strong> in the concrete implementation. For <code>numberStringSwap</code>, this means that we have to make the <code>radix</code> argument optional in the concrete implementation. This was done by specifying a default value for <code>radix</code>.</p>



<p>Not following these rules will result in a generic “Overload signature is not compatible with function definition” error.</p>



<p>Note that even though our fully defined function uses the <code>any</code> type for <code>value</code>, attempting to pass another type (like a boolean) for this parameter will cause TypeScript to throw an error because <strong>only the overloaded signatures are used for type checking</strong>. In a case where more than one signature would match a given call, the <em>first</em> overload listed in the source code will win:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">numberStringSwap</span><span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token builtin">any</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">any</span><span class="token punctuation">;</span>
<span class="token keyword">function</span> <span class="token function">numberStringSwap</span><span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>

<span class="token function">numberStringSwap</span><span class="token punctuation">(</span><span class="token string">'1234'</span><span class="token punctuation">)</span><span class="token punctuation">;</span></code></pre>



<p>Here, even though the second overload signature is more specific, the first will be used. This means that you always need to make sure your source code is ordered so more specific overloads won’t be shadowed by more general ones.</p>



<p>Function overloads also work within object type literals, interfaces, and classes:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">let</span> numberStringSwap<span class="token operator">:</span> <span class="token punctuation">{</span>
  <span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">,</span> radix<span class="token operator">?</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
  <span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre>



<p>Note that because we are defining a type and not creating an actual function declaration, the concrete implementation of numberStringSwap is omitted.</p>



<p>TypeScript also allows you to specify different return types when an exact string is provided as an argument to a function. For example, the DOM <a href="https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement" target="_blank" rel="noreferrer noopener nofollow">createElement</a> method could be typed like this:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token function">createElement</span><span class="token punctuation">(</span>tagName<span class="token operator">:</span> <span class="token string">'a'</span><span class="token punctuation">)</span><span class="token operator">:</span> HTMLAnchorElement<span class="token punctuation">;</span>
<span class="token function">createElement</span><span class="token punctuation">(</span>tagName<span class="token operator">:</span> <span class="token string">'abbr'</span><span class="token punctuation">)</span><span class="token operator">:</span> HTMLElement<span class="token punctuation">;</span>
<span class="token function">createElement</span><span class="token punctuation">(</span>tagName<span class="token operator">:</span> <span class="token string">'address'</span><span class="token punctuation">)</span><span class="token operator">:</span> HTMLElement<span class="token punctuation">;</span>
<span class="token function">createElement</span><span class="token punctuation">(</span>tagName<span class="token operator">:</span> <span class="token string">'area'</span><span class="token punctuation">)</span><span class="token operator">:</span> HTMLAreaElement<span class="token punctuation">;</span>
<span class="token comment">// ... etc.</span>
<span class="token function">createElement</span><span class="token punctuation">(</span>tagName<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span><span class="token operator">:</span> HTMLElement<span class="token punctuation">;</span></code></pre>



<p>This would let TypeScript know when <code>createElement('video')</code> is called, the return value will be an <code>HTMLVideoElement</code>, whereas when <code>createElement('a')</code> is called, the return value will be an <code>HTMLAnchorElement</code>.</p>



<h3>Strict function types</h3>



<p>By default, TypeScript is a bit lax when checking function type parameters. Consider the following example:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">Animal</span> <span class="token punctuation">{</span> <span class="token function">breathe</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> <span class="token punctuation">}</span> <span class="token punctuation">}</span>
<span class="token keyword">class</span> <span class="token class-name">Dog</span> <span class="token keyword">extends</span> <span class="token class-name">Animal</span> <span class="token punctuation">{</span> <span class="token function">bark</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span><span class="token punctuation">}</span> <span class="token punctuation">}</span>
<span class="token keyword">class</span> <span class="token class-name">Cat</span> <span class="token keyword">extends</span> <span class="token class-name">Animal</span> <span class="token punctuation">{</span> <span class="token function">meow</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span><span class="token punctuation">}</span> <span class="token punctuation">}</span>

<span class="token keyword">let</span> <span class="token function-variable function">f1</span><span class="token operator">:</span> <span class="token punctuation">(</span>x<span class="token operator">:</span> Animal<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token function-variable function">void</span> <span class="token operator">=</span> <span class="token punctuation">(</span>x<span class="token operator">:</span> Animal<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> x<span class="token punctuation">.</span><span class="token function">breathe</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">let</span> <span class="token function-variable function">f2</span><span class="token operator">:</span> <span class="token punctuation">(</span>x<span class="token operator">:</span> Dog<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token function-variable function">void</span> <span class="token operator">=</span> <span class="token punctuation">(</span>x<span class="token operator">:</span> Dog<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> x<span class="token punctuation">.</span><span class="token function">bark</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">let</span> <span class="token function-variable function">f3</span><span class="token operator">:</span> <span class="token punctuation">(</span>x<span class="token operator">:</span> Cat<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token function-variable function">void</span> <span class="token operator">=</span> <span class="token punctuation">(</span>x<span class="token operator">:</span> Cat<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> x<span class="token punctuation">.</span><span class="token function">meow</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

f1 <span class="token operator">=</span> f2<span class="token punctuation">;</span>
<span class="token keyword">const</span> c <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Cat</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token function">f1</span><span class="token punctuation">(</span>c<span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Runtime error</span></code></pre>



<p><code>Dog</code> is a type of animal, so the assignment <code>f1 = f2</code> is valid. However, now <code>f1</code> is a function that can only accept <code>Dog</code>, even though its type says it can accept any <code>Animal</code>. Trying to call <code>f1</code> on a <code>Cat</code> will generate a runtime error when the function tries to call <code>bark</code> on it.</p>



<p>TypeScript allows this situation because function arguments in TypeScript are <a href="https://github.com/Microsoft/TypeScript/wiki/FAQ#why-are-function-parameters-bivariant" target="_blank" rel="noreferrer noopener nofollow">bivariant</a>, which is unsound (as far as typing is concerned). The <code>strictFunctionTypes</code> compiler option can be enabled to flag this kind of unsound assignment.</p>



<h3>Rest Parameters</h3>



<p>Some functions may take an unspecified number of parameters. TypeScript allows expressing these using a rest parameter. For example, <code>Array.push</code> takes one or more parameters of the same type as the array. The example below shows the type for this function.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name"><span class="token builtin">Array</span><span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span></span> <span class="token punctuation">{</span>
    <span class="token function">push</span><span class="token punctuation">(</span><span class="token operator">...</span>args<span class="token operator">:</span> <span class="token constant">T</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>Without the use of rest parameters, you would need to write an overload for every number of arguments that the function needs to accept.</p>



<h3>Generic types</h3>



<p>TypeScript includes the concept of a <a href="https://en.wikipedia.org/wiki/Generic_programming" target="_blank" rel="noreferrer noopener nofollow">generic type</a>, which can be roughly thought of as a type that must include or reference another type in order to be complete. Two generic types that you’ve probably already used are <code>Array</code> and <code>Promise</code>.</p>



<p>The syntax of a generic value type is <code>GenericType&lt;a SpecificType&gt;</code>. For example, an “array of strings” type would be <code>Array&lt;a string&gt;</code>, and a “promise that resolves to a number” type would be <code>Promise&lt;a number&gt;</code>. Generic types may require more than one specific type, like <code>Converter&lt;a TInput, TOutput&gt;</code>, but this is uncommon. The placeholder types inside the angle brackets are called <em>type parameters</em>.</p>



<p>To explain how to create your own generic types, consider how an Array-like class might be typed:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Arrayish<span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span></span> <span class="token punctuation">{</span>
  <span class="token generic-function"><span class="token function">map</span><span class="token generic class-name"><span class="token operator">&lt;</span><span class="token constant">U</span><span class="token operator">&gt;</span></span></span><span class="token punctuation">(</span>
    <span class="token function-variable function">callback</span><span class="token operator">:</span> <span class="token punctuation">(</span>value<span class="token operator">:</span> <span class="token constant">T</span><span class="token punctuation">,</span> index<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">,</span> array<span class="token operator">:</span> Arrayish<span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token constant">U</span><span class="token punctuation">,</span>
    thisArg<span class="token operator">?</span><span class="token operator">:</span> <span class="token builtin">any</span>
  <span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">Array</span><span class="token operator">&lt;</span><span class="token constant">U</span><span class="token operator">&gt;</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>In this example, <code>Arrayish</code> is defined as a generic type with a single map method, which corresponds to the <code>Array#map</code> method from ECMAScript 5. The <code>map</code> method has a type parameter of its own, <code>U</code>, which is used to indicate that the return type of the callback function needs to be the same as the return type of the <code>map</code> call.</p>



<p>Actually using this type would look something like this:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">const</span> arrayOfStrings<span class="token operator">:</span> Arrayish<span class="token operator">&lt;</span><span class="token builtin">string</span><span class="token operator">&gt;</span> <span class="token operator">=</span> <span class="token punctuation">[</span> <span class="token string">'a'</span><span class="token punctuation">,</span> <span class="token string">'b'</span><span class="token punctuation">,</span> <span class="token string">'c'</span> <span class="token punctuation">]</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> arrayOfCharCodes<span class="token operator">:</span> Arrayish<span class="token operator">&lt;</span><span class="token builtin">number</span><span class="token operator">&gt;</span> <span class="token operator">=</span>
  arrayOfStrings<span class="token punctuation">.</span><span class="token function">map</span><span class="token punctuation">(</span>value <span class="token operator">=&gt;</span> value<span class="token punctuation">.</span><span class="token function">charCodeAt</span><span class="token punctuation">(</span><span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span></code></pre>



<p>Here, <code>arrayOfStrings</code> is defined as being an <code>Arrayish</code> containing strings, and <code>arrayOfCharCodes</code> is defined as being an <code>Arrayish</code> containing numbers. We call <code>map</code> on the array of strings, passing a callback function that returns numbers. If the callback returned a string instead of a number, the compiler would raise an error that the types were not compatible, because <code>arrayOfCharCodes</code> is explicitly typed.</p>



<p>Because arrays are an exceptionally common generic type, TypeScript provides a shorthand notation: <code>SpecificType[]</code>. Note, however, ambiguity can occasionally arise when using this shorthand. For example, is the type <code>() =&gt; boolean[]</code> an array of functions that return booleans, or is it a single function that returns an array of booleans? The answer is the latter; to represent the former, you would typically write <code>(() =&gt; boolean)[]</code>.</p>



<p>TypeScript also allows type parameters to be constrained to a specific type by using the <code>extends</code> keyword within the type parameter, like interface <code>PointPromise</code>. In this case, only a type that structurally matched <code>Point</code> could be used for <code>T</code>; trying to use something else, like <code>string</code>, would cause a type error.</p>



<p>Generic types may be given defaults, which can reduce boilerplate in many instances. For example, if we wanted a function that created an <code>Arrayish</code> based on the arguments passed but defaulted to <code>string</code> when no arguments are passed, we would write:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token generic-function"><span class="token function">createArrayish</span><span class="token generic class-name"><span class="token operator">&lt;</span><span class="token constant">T</span> <span class="token operator">=</span> <span class="token builtin">string</span><span class="token operator">&gt;</span></span></span><span class="token punctuation">(</span><span class="token operator">...</span>args<span class="token operator">:</span> <span class="token constant">T</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token operator">:</span> Arrayish<span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">return</span> args<span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<h3>Union types</h3>



<p><a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types" target="_blank" rel="noreferrer noopener">Union types</a> allow a parameter or variable to support more than one type. For example, if you wanted to have a convenience function like <code>document.getElementById</code> that could accept either a string ID or an element, like Dojo’s <code>byId</code> function, you could do this using a union type:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">byId</span><span class="token punctuation">(</span>element<span class="token operator">:</span> <span class="token builtin">string</span> <span class="token operator">|</span> Element<span class="token punctuation">)</span><span class="token operator">:</span> Element <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token keyword">typeof</span> element <span class="token operator">===</span> <span class="token string">'string'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> document<span class="token punctuation">.</span><span class="token function">getElementById</span><span class="token punctuation">(</span>element<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> element<span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>TypeScript is intelligent enough to contextually type the <code>element</code> variable inside the <code>if</code> block to be of type <code>string</code>, and to be of type <code>Element</code> in the <code>else</code> block. Code used to <a href="https://www.typescriptlang.org/docs/handbook/2/narrowing.html">narrow types</a> is typically referred to as a type guard; these will be discussed in more detail later in this article.</p>



<p>TypeScript 4.4 enhanced type guards such that <code>instanceof</code> and <code>typeof</code> checks can now be assigned to constant values. For example:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">performSomeWork</span><span class="token punctuation">(</span>someId<span class="token operator">:</span> <span class="token builtin">number</span> <span class="token operator">|</span> <span class="token keyword">undefined</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
   <span class="token keyword">const</span> hasSomeId <span class="token operator">=</span> <span class="token keyword">typeof</span> someId <span class="token operator">===</span> <span class="token string">"number"</span><span class="token punctuation">;</span>
 
   <span class="token keyword">if</span> <span class="token punctuation">(</span>hasSomeId<span class="token punctuation">)</span> <span class="token punctuation">{</span>
       <span class="token comment">// someId is a number</span>
       <span class="token comment">// perform some work</span>
   <span class="token punctuation">}</span>
 
   <span class="token comment">// someId is a number | undefined</span>
   <span class="token comment">// perform some work</span>
 
   <span class="token keyword">if</span> <span class="token punctuation">(</span>hasSomeId<span class="token punctuation">)</span> <span class="token punctuation">{</span>
       <span class="token comment">// someId is a number</span>
       <span class="token comment">// perform more work</span>
   <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>When declaring a generic type as a union type, a default type can be included.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">type</span> <span class="token class-name">Data<span class="token operator">&lt;</span><span class="token constant">T</span> <span class="token keyword">extends</span> <span class="token builtin">string</span> <span class="token operator">|</span> <span class="token builtin">number</span> <span class="token operator">=</span> <span class="token builtin">number</span><span class="token operator">&gt;</span></span> <span class="token operator">=</span> <span class="token punctuation">{</span> value<span class="token operator">:</span> <span class="token constant">T</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> data1<span class="token operator">:</span> Data <span class="token operator">=</span> <span class="token punctuation">{</span> value<span class="token operator">:</span> <span class="token number">3</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> data2<span class="token operator">:</span> Data <span class="token operator">=</span> <span class="token punctuation">{</span> value<span class="token operator">:</span> <span class="token string">'3'</span> <span class="token punctuation">}</span><span class="token punctuation">;</span> <span class="token comment">// error, value has to be a number</span>
<span class="token keyword">const</span> data3<span class="token operator">:</span> Data<span class="token operator">&lt;</span><span class="token builtin">number</span><span class="token operator">&gt;</span> <span class="token operator">=</span> <span class="token punctuation">{</span> value<span class="token operator">:</span> <span class="token number">3</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> data4<span class="token operator">:</span> Data<span class="token operator">&lt;</span><span class="token builtin">string</span><span class="token operator">&gt;</span> <span class="token operator">=</span> <span class="token punctuation">{</span> value<span class="token operator">:</span> <span class="token string">'3'</span><span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre>



<h3>Intersection types</h3>



<p>While union types indicate that a value may be one type or another, <a href="https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types" target="_blank" rel="noreferrer noopener">intersection types</a> indicate that a value will be a combination of multiple types; it must meet the contract of all of the member types. For example:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Foo</span> <span class="token punctuation">{</span>
  name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
  count<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">interface</span> <span class="token class-name">Bar</span> <span class="token punctuation">{</span>
  name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
  age<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">export</span> <span class="token keyword">type</span> <span class="token class-name">FooBar</span> <span class="token operator">=</span> Foo <span class="token operator">&amp;</span> Bar<span class="token punctuation">;</span></code></pre>



<p>A value of type FooBar must have <code>name</code>, <code>count</code>, and <code>age</code> properties.</p>



<p>TypeScript doesn’t require overlapping properties to have compatible types, so it’s possible to make unusable types:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Foo</span> <span class="token punctuation">{</span>
  count<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">interface</span> <span class="token class-name">Bar</span> <span class="token punctuation">{</span>
  count<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">export</span> <span class="token keyword">type</span> <span class="token class-name">FooBar2</span> <span class="token operator">=</span> Foo <span class="token operator">&amp;</span> Bar<span class="token punctuation">;</span></code></pre>



<p>The <code>count</code> property in <code>FooBar2</code> is of type <code>never</code> since a value can’t be both a string and a number, meaning no value can be assigned to it.</p>



<h3>Type aliases</h3>



<p>We saw earlier that <code>typeof</code> and <code>interfaces</code> were two ways to avoid having to code the full type of a value everywhere it’s needed. Another way to accomplish this is with <a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases" target="_blank" rel="noreferrer noopener">type aliases</a>. A type alias is just a reference to a specific type.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">import</span> <span class="token operator">*</span> <span class="token keyword">as</span> foo <span class="token keyword">from</span> <span class="token string">'./foo'</span><span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">Foo</span> <span class="token operator">=</span> foo<span class="token punctuation">.</span>Foo<span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">Bar</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">StringOrNumber</span> <span class="token operator">=</span> <span class="token builtin">string</span> <span class="token operator">|</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">PromiseOrValue<span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span></span> <span class="token operator">=</span> <span class="token constant">T</span> <span class="token operator">|</span> <span class="token builtin">Promise</span><span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span><span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">BarkingAnimal</span> <span class="token operator">=</span> Animal <span class="token operator">&amp;</span> <span class="token punctuation">{</span> <span class="token function">bark</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token keyword">void</span> <span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre>



<p>Type aliases are very similar to interfaces. They can be extended using the intersection operator, as with the <code>BarkingAnimal</code> type shown above. They can also be used as the base type for interfaces (except for aliases to union types).</p>



<p>Unlike interfaces, aliases aren’t subject to <a href="https://www.typescriptlang.org/docs/handbook/declaration-merging.html#merging-interfaces" target="_blank" rel="noreferrer noopener nofollow">declaration merging</a>. When an interface is defined multiple times in a single scope, the declarations will be merged into a single interface. A type alias, on the other hand, is a named entity, like a variable. As with variables, <strong>type declarations are block scoped</strong>, and you can’t declare two types with the same name in the same scope.</p>



<h3>Mapped types</h3>



<p>Mapped types allow for the creation of new types based on existing types by mapping properties of an existing type to a new type. Consider the type <code>Stringify</code> below; <code>Stringify</code> will have all the same properties as T, but those properties will all have values of type string.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">type</span> <span class="token class-name">Stringify<span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span></span> <span class="token operator">=</span> <span class="token punctuation">{</span>
  <span class="token punctuation">[</span><span class="token constant">P</span> <span class="token keyword">in</span> <span class="token keyword">keyof</span> <span class="token constant">T</span><span class="token punctuation">]</span><span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>
 
<span class="token keyword">interface</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> <span class="token punctuation">}</span>
<span class="token keyword">type</span> <span class="token class-name">StringPoint</span> <span class="token operator">=</span> Stringify<span class="token operator">&lt;</span>Point<span class="token operator">&gt;</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> pointA<span class="token operator">:</span> StringPoint <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token string">'4'</span><span class="token punctuation">,</span> <span class="token constant">Y</span><span class="token operator">:</span> <span class="token string">'3'</span> <span class="token punctuation">}</span><span class="token punctuation">;</span> <span class="token comment">// valid</span></code></pre>



<p>Note that mapped types only affect <em>types</em>, not values; the <code>Stringify</code> type above won’t actually transform an object of arbitrary values into an object of strings.</p>



<p>TypeScript 2.8 added the ability to add or remove <code>readonly</code> or <code>?</code> modifiers from mapped properties. This is done using + and – to indicate whether the modifier should be added or removed.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">type</span> <span class="token class-name">MutableRequired<span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span></span> <span class="token operator">=</span> <span class="token punctuation">{</span> <span class="token operator">-</span><span class="token keyword">readonly</span> <span class="token punctuation">[</span><span class="token constant">P</span> <span class="token keyword">in</span> <span class="token keyword">keyof</span> <span class="token constant">T</span><span class="token punctuation">]</span><span class="token operator">-</span><span class="token operator">?</span><span class="token operator">:</span> <span class="token constant">T</span><span class="token punctuation">[</span><span class="token constant">P</span><span class="token punctuation">]</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">ReadonlyPartial<span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span></span> <span class="token operator">=</span> <span class="token punctuation">{</span> <span class="token operator">+</span><span class="token keyword">readonly</span> <span class="token punctuation">[</span><span class="token constant">P</span> <span class="token keyword">in</span> <span class="token keyword">keyof</span> <span class="token constant">T</span><span class="token punctuation">]</span><span class="token operator">+</span><span class="token operator">?</span><span class="token operator">:</span> <span class="token constant">T</span><span class="token punctuation">[</span><span class="token constant">P</span><span class="token punctuation">]</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
    
<span class="token keyword">interface</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span> <span class="token keyword">readonly</span> x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span> <span class="token punctuation">}</span>
<span class="token keyword">const</span> pointA<span class="token operator">:</span> ReadonlyPartial<span class="token operator">&lt;</span>Point<span class="token operator">&gt;</span> <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token number">4</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
pointA<span class="token punctuation">.</span>y <span class="token operator">=</span> <span class="token number">3</span><span class="token punctuation">;</span> <span class="token comment">// Error: readonly</span>
<span class="token keyword">const</span> pointB<span class="token operator">:</span> MutableRequired<span class="token operator">&lt;</span>Point<span class="token operator">&gt;</span> <span class="token operator">=</span> <span class="token punctuation">{</span> x<span class="token operator">:</span> <span class="token number">4</span><span class="token punctuation">,</span> y<span class="token operator">:</span> <span class="token number">3</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
pointB<span class="token punctuation">.</span>x <span class="token operator">=</span> <span class="token number">2</span><span class="token punctuation">;</span> <span class="token comment">// valid</span></code></pre>



<p>In the example above, <code>MutableRequired</code> makes all properties of its source type non-optional and writable, whereas <code>ReadonlyPartial</code> makes all properties optional and readonly.</p>



<p>TypeScript 3.1 introduced the ability to map over a tuple type and return a new tuple type. Consider the following example where a tuple type <code>Point</code> is defined. Suppose that in some cases points will actually be Promises that resolve to <code>Point</code> objects. TypeScript allows for the creation of the latter type from the former:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">type</span> <span class="token class-name">ToPromise<span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span></span> <span class="token operator">=</span> <span class="token punctuation">{</span> <span class="token punctuation">[</span><span class="token constant">K</span> <span class="token keyword">in</span> <span class="token keyword">typeof</span> <span class="token constant">T</span><span class="token punctuation">]</span><span class="token operator">:</span> <span class="token builtin">Promise</span><span class="token operator">&lt;</span><span class="token constant">T</span><span class="token punctuation">[</span><span class="token constant">K</span><span class="token punctuation">]</span><span class="token operator">&gt;</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">Point</span> <span class="token operator">=</span> <span class="token punctuation">[</span> <span class="token builtin">number</span><span class="token punctuation">,</span> <span class="token builtin">number</span> <span class="token punctuation">]</span><span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">PromisePoint</span> <span class="token operator">=</span> ToPromise<span class="token operator">&lt;</span>Point<span class="token operator">&gt;</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> point<span class="token operator">:</span> PromisePoint <span class="token operator">=</span>
  <span class="token punctuation">[</span> <span class="token builtin">Promise</span><span class="token punctuation">.</span><span class="token function">resolve</span><span class="token punctuation">(</span><span class="token number">2</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token builtin">Promise</span><span class="token punctuation">.</span><span class="token function">resolve</span><span class="token punctuation">(</span><span class="token number">3</span><span class="token punctuation">)</span> <span class="token punctuation">]</span><span class="token punctuation">;</span> <span class="token comment">// valid</span></code></pre>



<p>TypeScript 4.1 continued to expand the capability of mapped types by adding the ability to remap the keys into a new type using the as keyword. This feature allows, among other things, the acceptable keys to be derived from the keys of a generic source using template literal types. Consider the following interface:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Person</span> <span class="token punctuation">{</span>
   name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
   age<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
   location<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>By combining template literal types and re-mapping, we can create a generic getter type whose keys represent accessor methods for the source type’s fields.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">type</span> <span class="token class-name">Getters<span class="token operator">&lt;</span><span class="token constant">T</span><span class="token operator">&gt;</span></span> <span class="token operator">=</span> <span class="token punctuation">{</span>
   <span class="token punctuation">[</span><span class="token constant">K</span> <span class="token keyword">in</span> <span class="token keyword">keyof</span> <span class="token constant">T</span> <span class="token keyword">as</span> <span class="token template-string"><span class="token template-punctuation string">`</span><span class="token string">get</span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span>Capitalize<span class="token operator">&lt;</span><span class="token builtin">string</span> <span class="token operator">&amp;</span> <span class="token constant">K</span><span class="token operator">&gt;</span><span class="token interpolation-punctuation punctuation">}</span></span><span class="token template-punctuation string">`</span></span><span class="token punctuation">]</span><span class="token operator">:</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token constant">T</span><span class="token punctuation">[</span><span class="token constant">K</span><span class="token punctuation">]</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>
<span class="token keyword">type</span> <span class="token class-name">LazyPerson</span> <span class="token operator">=</span> Getters<span class="token operator">&lt;</span>Person<span class="token operator">&gt;</span><span class="token punctuation">;</span></code></pre>



<p>Certain mapped type patterns are so common that they’ve become built-in types in TypeScript:</p>



<ul>
<li><a href="https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype" target="_blank" rel="noreferrer noopener">Partial&lt;T&gt;</a> – constructs a type with all the properties of T set to optional</li>



<li><a href="https://www.typescriptlang.org/docs/handbook/utility-types.html#requiredtype" target="_blank" rel="noreferrer noopener">Required&lt;T&gt;</a> – constructs a type with all the properties of T set to required</li>



<li><a href="https://www.typescriptlang.org/docs/handbook/utility-types.html#readonlytype" target="_blank" rel="noreferrer noopener">Readonly&lt;T&gt;</a> – constructs a type with all the properties of T set to readonly</li>



<li><a href="https://www.typescriptlang.org/docs/handbook/utility-types.html#recordkeys-type" target="_blank" rel="noreferrer noopener">Record&lt;K, T&gt;</a> – constructs a type with property names from K, where each property has type T</li>



<li><a href="https://www.typescriptlang.org/docs/handbook/utility-types.html#picktype-keys" target="_blank" rel="noreferrer noopener">Pick&lt;T, K&gt;</a> – constructs a type with just the properties from T specified by K</li>



<li><a href="https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys" target="_blank" rel="noreferrer noopener">Omit&lt;T, K&gt;</a> – constructs a type with all the properties from T <em>except</em> those specified by K</li>
</ul>



<h3>Conditional types</h3>



<p><a href="https://www.typescriptlang.org/docs/handbook/2/conditional-types.html" target="_blank" rel="noreferrer noopener">Conditional types</a> allow for a type to be set dynamically based on a provided condition. All conditional types follow the same format: <code>T extends U ? X : Y</code>. This may look familiar since it uses the same syntax as a JavaScript <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator" target="_blank" rel="noreferrer noopener nofollow">ternary</a> statement. What this statement means is if <code>T</code> is assignable to <code>U</code>, then set the type to <code>X</code>. Otherwise, set the type to <code>Y</code>.</p>



<p>This may seem like a very simple concept, but it can dramatically simplify complex typings. Consider the following example where we would like to define types for a function that accepts either a number or a string.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">declare</span> <span class="token keyword">function</span> <span class="token function">addOrConcat</span><span class="token punctuation">(</span>x<span class="token operator">:</span> <span class="token builtin">number</span> <span class="token operator">|</span> <span class="token builtin">string</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">number</span> <span class="token operator">|</span> <span class="token builtin">string</span><span class="token punctuation">;</span></code></pre>



<p>The types here are fine but they do not truly convey the meaning or intent of the code. Presumably, if the argument is a <code>number</code> then the return type will also be <code>number</code>, and likewise with <code>string</code>. To correct this, we can use function overloading</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">declare</span> <span class="token keyword">function</span> <span class="token function">addOrConcat</span><span class="token punctuation">(</span>x<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
<span class="token keyword">declare</span> <span class="token keyword">function</span> <span class="token function">addOrConcat</span><span class="token punctuation">(</span>x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token keyword">declare</span> <span class="token keyword">function</span> <span class="token function">addOrConcat</span><span class="token punctuation">(</span>x<span class="token operator">:</span> <span class="token builtin">number</span> <span class="token operator">|</span> <span class="token builtin">string</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">number</span> <span class="token operator">|</span> <span class="token builtin">string</span><span class="token punctuation">;</span></code></pre>



<p>However, this is a little verbose and can be tedious to change in the future. Enter conditional types!</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">declare</span> <span class="token keyword">function</span> <span class="token generic-function"><span class="token function">addOrConcat</span><span class="token generic class-name"><span class="token operator">&lt;</span><span class="token constant">T</span> <span class="token keyword">extends</span> <span class="token builtin">number</span> <span class="token operator">|</span> <span class="token builtin">string</span><span class="token operator">&gt;</span></span></span><span class="token punctuation">(</span>x<span class="token operator">:</span> <span class="token constant">T</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token constant">T</span> <span class="token keyword">extends</span> <span class="token class-name"><span class="token builtin">number</span></span> <span class="token operator">?</span> <span class="token builtin">number</span> <span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span></code></pre>



<p>This function signature is generic, stating that T will either be a <code>number</code> or a <code>string</code>. A conditional type is used to determine the return type; if the function argument is a <code>number</code>, the function return type is number, otherwise it’s <code>string</code>.</p>



<p>The <code>infer</code> keyword can be used in conditional types to introduce a type variable that the TypeScript compiler will infer from its context. For example, you could write a function that infers the type of a tuple from its members and returns the first element as that type.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token generic-function"><span class="token function">first</span><span class="token generic class-name"><span class="token operator">&lt;</span><span class="token constant">T</span> <span class="token keyword">extends</span> <span class="token punctuation">[</span><span class="token builtin">any</span><span class="token punctuation">,</span> <span class="token builtin">any</span><span class="token punctuation">]</span><span class="token operator">&gt;</span></span></span><span class="token punctuation">(</span>pair<span class="token operator">:</span> <span class="token constant">T</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token constant">T</span> <span class="token keyword">extends</span> <span class="token punctuation">[</span><span class="token keyword">infer</span> <span class="token constant">U</span><span class="token punctuation">,</span> <span class="token keyword">infer</span> <span class="token constant">U</span><span class="token punctuation">]</span> <span class="token operator">?</span> <span class="token constant">U</span> <span class="token operator">:</span> <span class="token builtin">any</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> pair<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
 
<span class="token function">first</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">,</span> <span class="token string">'foo'</span><span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Type will be string | number</span>
<span class="token function">first</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Type will be number</span></code></pre>



<h3>Type guards</h3>



<p><a href="https://www.typescriptlang.org/docs/handbook/2/narrowing.html#typeof-type-guards" target="_blank" rel="noreferrer noopener">Type guards</a> allow for narrowing of types within a conditional block. This is essential when working with types that could be unions of two or more types, or where the type is not known until runtime. To do this in a way that is also compatible with the JavaScript code that will be run at runtime, the type system ties into the <a href="https://www.typescriptlang.org/docs/handbook/2/narrowing.html#typeof-type-guards" target="_blank" rel="noreferrer noopener"><code>typeof</code></a>, <a href="https://www.typescriptlang.org/docs/handbook/2/narrowing.html#instanceof-narrowing" target="_blank" rel="noreferrer noopener"><code>instanceof</code></a>, and <a href="https://www.typescriptlang.org/docs/handbook/2/narrowing.html#the-in-operator-narrowing"><code>in</code></a> (as of TS 2.7) operators, among <a href="https://www.typescriptlang.org/docs/handbook/2/narrowing.html" target="_blank" rel="noreferrer noopener">other ways of narrowing types</a>. Inside a conditional block using one of these checks, it is guaranteed that the value checked is of the specified type, and methods that would exist on that type can be used safely.</p>



<p><strong><code>typeof</code></strong><strong> and </strong><strong><code>instanceof</code></strong></p>



<p>TypeScript will use the JavaScript <code>typeof</code> and <code>instanceof</code> operators as type guards.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">lower</span><span class="token punctuation">(</span>x<span class="token operator">:</span> <span class="token builtin">string</span> <span class="token operator">|</span> <span class="token builtin">string</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token keyword">typeof</span> x <span class="token operator">===</span> <span class="token string">'string'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token comment">// x is guaranteed to be a string, so we can use toLowerCase</span>

  <span class="token keyword">return</span> x<span class="token punctuation">.</span><span class="token function">toLowerCase</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
  <span class="token comment">// x is definitely an array of strings, so we can use reduce</span>
  <span class="token keyword">return</span> x<span class="token punctuation">.</span><span class="token function">reduce</span><span class="token punctuation">(</span>
      <span class="token punctuation">(</span>val<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">,</span> next<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> val <span class="token operator">+=</span> <span class="token template-string"><span class="token template-punctuation string">`</span><span class="token string">, </span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span>next<span class="token punctuation">.</span><span class="token function">toLowerCase</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token interpolation-punctuation punctuation">}</span></span><span class="token template-punctuation string">`</span></span><span class="token punctuation">,</span> <span class="token string">''</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>

<span class="token keyword">function</span> <span class="token function">clearElement</span><span class="token punctuation">(</span>element<span class="token operator">:</span> <span class="token builtin">string</span> <span class="token operator">|</span> HTMLElement<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span>element <span class="token keyword">instanceof</span> <span class="token class-name">HTMLElement</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token comment">// element is guaranteed to be an HTMLElement in here</span>
    <span class="token comment">// so we can access its innerHTML property</span>
    element<span class="token punctuation">.</span>innerHTML <span class="token operator">=</span> <span class="token string">''</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
    <span class="token comment">// element is a string in here so we can pass that to querySelector</span>
    <span class="token keyword">const</span> el <span class="token operator">=</span> document<span class="token punctuation">.</span><span class="token function">querySelector</span><span class="token punctuation">(</span>element<span class="token punctuation">)</span><span class="token punctuation">;</span>
    el <span class="token operator">&amp;&amp;</span> el<span class="token punctuation">.</span>innerHTML <span class="token operator">=</span> <span class="token string">''</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>TypeScript understands, based on the result of a <code>typeof</code> or <code>instanceof</code> check, what the type of <code>x</code> must be in each part of an <code>if/else</code> statement.</p>



<p><strong><code>in</code></strong></p>



<p>This type guard narrows the type within a conditional by checking if a property exists on the variable. If the result is <code>true</code>, the variable type will be narrowed to match the type that contains the value checked on.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span>
  x<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
  y<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">interface</span> <span class="token class-name">Point3d</span> <span class="token keyword">extends</span> <span class="token class-name">Point</span> <span class="token punctuation">{</span>
  z<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">function</span> <span class="token function">plot</span><span class="token punctuation">(</span>point<span class="token operator">:</span> Point<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token string">'z'</span> <span class="token keyword">in</span> point<span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token comment">// point is a Point3D</span>
  <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
    <span class="token comment">// point is a Point</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>As of TypeScript 4.5, template string types can be used as type guards.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Data</span> <span class="token punctuation">{</span>
    type<span class="token operator">:</span> $<span class="token punctuation">{</span><span class="token builtin">string</span><span class="token punctuation">}</span>Result<span class="token punctuation">;</span>
    data<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token keyword">interface</span> <span class="token class-name">Failure</span> <span class="token punctuation">{</span>
    type<span class="token operator">:</span> $<span class="token punctuation">{</span><span class="token builtin">string</span><span class="token punctuation">}</span>Error<span class="token punctuation">;</span>
    message<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token keyword">function</span> <span class="token function">handleResponse</span><span class="token punctuation">(</span>response<span class="token operator">:</span> Data <span class="token operator">|</span> Failure<span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">if</span> <span class="token punctuation">(</span>response<span class="token punctuation">.</span>type <span class="token operator">===</span> <span class="token string">'DatabaseResult'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token comment">// Response type is narrowed to Data.</span>
        <span class="token function">handleDatabaseData</span><span class="token punctuation">(</span>response<span class="token punctuation">.</span>data<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span>response<span class="token punctuation">.</span>type <span class="token operator">===</span> <span class="token string">'DatabaseError'</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token comment">// Response type is narrowed to Failure.</span>
        <span class="token function">handleDatabaseError</span><span class="token punctuation">(</span>response<span class="token punctuation">.</span>message<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>TypeScript 4.7+ will narrow the type of a bracketed element.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">type</span> <span class="token class-name">Data</span> <span class="token operator">=</span>
   <span class="token operator">|</span> <span class="token punctuation">{</span> kind<span class="token operator">:</span> <span class="token string">"NumberData"</span><span class="token punctuation">,</span> value<span class="token operator">:</span> <span class="token builtin">number</span> <span class="token punctuation">}</span>
   <span class="token operator">|</span> <span class="token punctuation">{</span> kind<span class="token operator">:</span> <span class="token string">"StringData"</span><span class="token punctuation">,</span> value<span class="token operator">:</span> <span class="token builtin">string</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>
 
<span class="token keyword">function</span> <span class="token function">processData</span><span class="token punctuation">(</span>data<span class="token operator">:</span> Data<span class="token punctuation">)</span> <span class="token punctuation">{</span>
   <span class="token keyword">if</span> <span class="token punctuation">(</span>data<span class="token punctuation">.</span>kind <span class="token operator">===</span> <span class="token string">"NumberData"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
       <span class="token keyword">let</span> num <span class="token operator">=</span> data<span class="token punctuation">.</span>value<span class="token punctuation">;</span> <span class="token comment">// value is a number</span>
   <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
       <span class="token keyword">let</span> str <span class="token operator">=</span> data<span class="token punctuation">.</span>value<span class="token punctuation">;</span> <span class="token comment">// value is a string</span>
   <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<h3>Type predicates</h3>



<p>You can also create functions that return <a href="https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates" target="_blank" rel="noreferrer noopener">type predicates</a>, explicitly indicating the type of a value.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">function</span> <span class="token function">isDog</span><span class="token punctuation">(</span>animal<span class="token operator">:</span> Animal<span class="token punctuation">)</span><span class="token operator">:</span> animal <span class="token keyword">is</span> Dog <span class="token punctuation">{</span>
  <span class="token keyword">return</span> <span class="token keyword">typeof</span> <span class="token punctuation">(</span>animal <span class="token keyword">as</span> Dog<span class="token punctuation">)</span><span class="token punctuation">.</span>bark <span class="token operator">===</span> <span class="token string">'function'</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">isDog</span><span class="token punctuation">(</span>someAnimal<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  someAnimal<span class="token punctuation">.</span><span class="token function">bark</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// valid</span>
<span class="token punctuation">}</span></code></pre>



<p>The predicate <code>animal</code> is <code>Dog</code> says that if the function returns true, then the function’s argument is explicitly of type <code>Dog</code>.</p>



<h3>Classes</h3>



<p>For the most part, classes in TypeScript are similar to classes in standard JavaScript, but there are a few differences to allow classes to be properly typed.</p>



<p>TypeScript allows class fields to be explicitly declared so that the compiler will know what properties are valid for a class. Class fields can also be declared as <code>protected</code> and <code>private</code>, and as of TS 3.8 may also use <a href="https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#ecmascript-private-fields" target="_blank" rel="noreferrer noopener">ECMAScript private fields</a>.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">Animal</span> <span class="token punctuation">{</span>
  <span class="token keyword">protected</span> _happy<span class="token operator">:</span> <span class="token builtin">boolean</span><span class="token punctuation">;</span>
  name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
  #secretId<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>

  <span class="token function">constructor</span><span class="token punctuation">(</span>name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>name <span class="token operator">=</span> name<span class="token punctuation">;</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>#secretId <span class="token operator">=</span> Math<span class="token punctuation">.</span><span class="token function">random</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token function">pet</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token keyword">void</span> <span class="token punctuation">{</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>_happy <span class="token operator">=</span> <span class="token boolean">true</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>Note that TypeScript’s <code>private</code> modifier is not related to ECMAScript private fields, which are denoted with a hash sign (e.g., <code>#privateField</code>). Private TS fields are only private during compilation; at runtime they are accessible just like any normal class field. This is why the JavaScript convention of prefixing private fields with underscores is still commonly seen in TS code. ECMAScript private fields, on the other hand, have “hard” privacy and are completely inaccessible outside of a class at runtime.</p>



<p>ECMAScript private fields provide what is known as a “brand check”.&nbsp; A class’s code can try to access a private field in an object.&nbsp; A value will be retrieved if the object was created using the class’s constructor, otherwise, an exception will be thrown.&nbsp; EC2022 and TypeScript 4.5 allows the <code>in</code> operator to check for the existence of a private field without generating an exception.&nbsp; For example, in the <code>Animal</code> class above, the following static method could be added:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript">  <span class="token keyword">static</span> <span class="token function">isAnimal</span><span class="token punctuation">(</span>object<span class="token operator">:</span> <span class="token builtin">any</span><span class="token punctuation">)</span><span class="token operator">:</span> object <span class="token keyword">is</span> Animal <span class="token punctuation">{</span>
    <span class="token keyword">return</span> #secretId <span class="token keyword">in</span> object<span class="token punctuation">;</span>
  <span class="token punctuation">}</span></code></pre>



<p>TypeScript also allows class fields to use a <code>static</code> modifier, which indicates that they are actually properties on the class itself rather than instance properties (on the class’s prototype).</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">Dog</span> <span class="token keyword">extends</span> <span class="token class-name">Animal</span> <span class="token punctuation">{</span>
  <span class="token keyword">static</span> <span class="token function">isDogLike</span><span class="token punctuation">(</span>object<span class="token operator">:</span> <span class="token builtin">any</span><span class="token punctuation">)</span><span class="token operator">:</span> object <span class="token keyword">is</span> Dog <span class="token punctuation">{</span>
    <span class="token keyword">return</span> object<span class="token punctuation">.</span>bark <span class="token operator">&amp;&amp;</span> object<span class="token punctuation">.</span>pet<span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>

<span class="token keyword">if</span> <span class="token punctuation">(</span>Dog<span class="token punctuation">.</span><span class="token function">isDogLike</span><span class="token punctuation">(</span>someAnimal<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  someAnimal<span class="token punctuation">.</span><span class="token function">bark</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>As of TypeScript 4.4, you can use static blocks to initialize static class fields. This is particularly useful if you need to perform some initialization logic on your fields.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">Configuration</span> <span class="token punctuation">{</span>
   <span class="token keyword">static</span> host<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
   <span class="token keyword">static</span> port<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>

   <span class="token keyword">static</span> <span class="token punctuation">{</span>
       <span class="token keyword">try</span> <span class="token punctuation">{</span>
           <span class="token keyword">const</span> config <span class="token operator">=</span> <span class="token function">parseConfigFile</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
           Configuration<span class="token punctuation">.</span>host <span class="token operator">=</span> config<span class="token punctuation">[</span><span class="token string">"host"</span><span class="token punctuation">]</span><span class="token punctuation">;</span>
           Configuration<span class="token punctuation">.</span>port <span class="token operator">=</span> config<span class="token punctuation">[</span><span class="token string">"port"</span><span class="token punctuation">]</span><span class="token punctuation">;</span>
       <span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token punctuation">{</span>
           Configuration<span class="token punctuation">.</span>host <span class="token operator">=</span> <span class="token string">"default host"</span><span class="token punctuation">;</span>
           Configuration<span class="token punctuation">.</span>port <span class="token operator">=</span> <span class="token number">8080</span><span class="token punctuation">;</span>
       <span class="token punctuation">}</span>
   <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>Static blocks are also a great way to initialize private static fields that can’t be accessed outside of the class.</p>



<p>Properties may be declared <code>readonly</code> to indicate that they can only be set when an object is created. This is essentially <code>const</code> for object properties.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">Dog</span> <span class="token keyword">extends</span> <span class="token class-name">Animal</span> <span class="token punctuation">{</span>
  <span class="token keyword">readonly</span> breed<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>

  <span class="token function">constructor</span><span class="token punctuation">(</span>name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">,</span> breed<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">super</span><span class="token punctuation">(</span>name<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>breed <span class="token operator">=</span> breed<span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>You must call <code>super</code> in a constructor before any references to <code>this</code>.&nbsp; In TypeScript 4.6+, you are allowed to run code before a call to <code>super</code> as long as that code does not reference <code>this</code>.</p>



<p>Classes also support getters and setters for properties. A getter lets you compute a value to return as the property value, while a setter lets you run arbitrary code when the property is set. For example, the above animal class could be extended with a <code>status</code> getter that derives a status message from its other properties.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">Animal</span> <span class="token punctuation">{</span>
  <span class="token keyword">protected</span> _happy<span class="token operator">:</span> <span class="token builtin">boolean</span><span class="token punctuation">;</span>
  name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
  #secretId<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">;</span>

  <span class="token function">constructor</span><span class="token punctuation">(</span>name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>name <span class="token operator">=</span> name<span class="token punctuation">;</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>#secretId <span class="token operator">=</span> Math<span class="token punctuation">.</span><span class="token function">random</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token function">pet</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token keyword">void</span> <span class="token punctuation">{</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>_happy <span class="token operator">=</span> <span class="token boolean">true</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token keyword">get</span> <span class="token function">status</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">string</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token template-string"><span class="token template-punctuation string">`</span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span><span class="token keyword">this</span><span class="token punctuation">.</span>name<span class="token interpolation-punctuation punctuation">}</span></span><span class="token string"> </span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span><span class="token keyword">this</span><span class="token punctuation">.</span>_happy <span class="token operator">?</span> <span class="token string">'is'</span> <span class="token operator">:</span> <span class="token string">'is not'</span><span class="token interpolation-punctuation punctuation">}</span></span><span class="token string"> happy</span><span class="token template-punctuation string">`</span></span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
<span class="token keyword">const</span> animal <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Animal</span><span class="token punctuation">(</span><span class="token string">'Spike'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> status <span class="token operator">=</span> animal<span class="token punctuation">.</span>status<span class="token punctuation">;</span> <span class="token comment">// status = 'Spike is not happy';</span></code></pre>



<p>Properties may also be initialized in a class definition. The initial value of a property can be <strong>any assignment expression</strong>, not just a static value, and will be executed every time a new instance is created:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">DomesticatedDog</span> <span class="token keyword">extends</span> <span class="token class-name">Dog</span> <span class="token punctuation">{</span>
  age <span class="token operator">=</span> Math<span class="token punctuation">.</span><span class="token function">random</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">*</span> <span class="token number">20</span><span class="token punctuation">;</span>
  collarType <span class="token operator">=</span> <span class="token string">'leather'</span><span class="token punctuation">;</span>
  toys<span class="token operator">:</span> Toy<span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>Since initializers are executed for each new instance, you don’t have to worry about objects or arrays being shared across instances as you would if they were specified on an object prototype, which alleviates a common point of confusion for people using JavaScript “class-like” inheritance libraries that specify properties on the prototype.</p>



<p>When using constructors, properties may be declared and initialized through the constructor definition by prefixing parameters with an access modifier and/or <code>readonly</code>:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">DomesticatedDog</span> <span class="token keyword">extends</span> <span class="token class-name">Dog</span> <span class="token punctuation">{</span>
  toys<span class="token operator">:</span> Toy<span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">;</span>

  <span class="token function">constructor</span><span class="token punctuation">(</span>
    <span class="token keyword">public</span> name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">,</span>
    <span class="token keyword">readonly</span> <span class="token keyword">public</span> age<span class="token operator">:</span> <span class="token builtin">number</span><span class="token punctuation">,</span>
    <span class="token keyword">public</span> collarType<span class="token operator">:</span> <span class="token builtin">string</span>
  <span class="token punctuation">)</span> <span class="token punctuation">{</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>Here the <code>name</code>, <code>age</code>, and <code>collarType</code> constructor parameters will become class properties and will be initialized with the parameter values.</p>



<p>As of TypeScript 4.0, class property types can also be inferred from their assignments in the constructor. Take the following example:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">Animal</span> <span class="token punctuation">{</span>
 sharpTeeth<span class="token punctuation">;</span> <span class="token comment">// &lt;-- no type here! 😱</span>
 <span class="token function">constructor</span><span class="token punctuation">(</span>fangs <span class="token operator">=</span> <span class="token number">2</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">this</span><span class="token punctuation">.</span>sharpTeeth <span class="token operator">=</span> fangs<span class="token punctuation">;</span>
 <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>Prior to TypeScript 4.0, this would cause <code>sharpTeeth</code> to be typed as any (or as an error if using a strict option). Now, however, TypeScript can infer <code>sharpTeeth</code> is the same type as fangs, which is a number.</p>



<h3>Typing this</h3>



<p>TypeScript can infer the type of <code>this</code> in normal class methods. In places where it can’t be inferred, such as nested functions, <code>this</code> will default to the <code>any</code> type. The type of <code>this</code> can be specified by providing a fake first parameter in a function type.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">Dog</span> <span class="token punctuation">{</span>
  name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
  <span class="token function-variable function">bark</span><span class="token operator">:</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token keyword">void</span><span class="token punctuation">;</span>

  <span class="token function">constructor</span><span class="token punctuation">(</span>name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>name <span class="token operator">=</span> name<span class="token punctuation">;</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>bark <span class="token operator">=</span> <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">createBarkFunction</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token function">createBarkFunction</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token keyword">function</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token operator">:</span> Dog<span class="token punctuation">)</span> <span class="token punctuation">{</span>
      <span class="token builtin">console</span><span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token template-string"><span class="token template-punctuation string">`</span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span><span class="token keyword">this</span><span class="token punctuation">.</span>name<span class="token interpolation-punctuation punctuation">}</span></span><span class="token string"> says hi!</span><span class="token template-punctuation string">`</span></span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>Setting the <code>noImplicitThis</code> compiler flag will cause TypeScript to emit a compiler error whenever <code>this</code> would default to the <code>any</code> type.</p>



<h3>Multiple inheritance and mixins</h3>



<p>In TypeScript, interfaces can extend other interfaces <em>and</em> classes, which can be useful when composing complex types, especially if you are used to writing mixins and using multiple inheritance:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">interface</span> <span class="token class-name">Chimera</span> <span class="token keyword">extends</span> <span class="token class-name">Dog</span><span class="token punctuation">,</span> Lion<span class="token punctuation">,</span> Monsterish <span class="token punctuation">{</span><span class="token punctuation">}</span>

<span class="token keyword">class</span> <span class="token class-name">MyChimera</span> <span class="token keyword">implements</span> <span class="token class-name">Chimera</span> <span class="token punctuation">{</span>
  <span class="token function-variable function">bark</span><span class="token operator">:</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
  <span class="token function-variable function">roar</span><span class="token operator">:</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
  <span class="token function">terrorize</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token keyword">void</span> <span class="token punctuation">{</span>
    <span class="token comment">// ...</span>
  <span class="token punctuation">}</span>
  <span class="token comment">// ...</span>
<span class="token punctuation">}</span>

MyChimera<span class="token punctuation">.</span>prototype<span class="token punctuation">.</span>bark <span class="token operator">=</span> Dog<span class="token punctuation">.</span>prototype<span class="token punctuation">.</span>bark<span class="token punctuation">;</span>
MyChimera<span class="token punctuation">.</span>prototype<span class="token punctuation">.</span>roar <span class="token operator">=</span> Lion<span class="token punctuation">.</span>prototype<span class="token punctuation">.</span>roar<span class="token punctuation">;</span></code></pre>



<p>In this example, two classes (<code>Dog</code> and <code>Lion</code>) and an interface (<code>Monsterish</code>) have been combined into a new <code>Chimera</code> interface. The <code>MyChimera</code> class implements that interface, delegating back to the original classes for function implementations. Note that the <code>bark</code> and <code>roar</code> methods are actually defined as properties rather than methods; this allows the interface to be “fully implemented” by the class despite the concrete implementation not actually existing within the class definition. This is one of the more advanced use cases for classes in TypeScript, but it enables extremely robust and efficient code reuse when used properly.</p>



<p>TypeScript is also able to handle typings for ES2015 <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes#Mix-ins" target="_blank" rel="noreferrer noopener nofollow">mixin classes</a>. A mixin is a function that takes a constructor and returns a new class (the mixin class) that is an extension of the constructor.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">class</span> <span class="token class-name">Dog</span> <span class="token keyword">extends</span> <span class="token class-name">Animal</span> <span class="token punctuation">{</span>
  name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
 
  <span class="token function">constructor</span><span class="token punctuation">(</span>name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span>name <span class="token operator">=</span> name<span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
 
<span class="token keyword">type</span> <span class="token class-name">Constructor<span class="token operator">&lt;</span><span class="token constant">T</span> <span class="token operator">=</span> <span class="token punctuation">{</span><span class="token punctuation">}</span><span class="token operator">&gt;</span></span> <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token punctuation">(</span><span class="token operator">...</span>args<span class="token operator">:</span> <span class="token builtin">any</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token constant">T</span><span class="token punctuation">;</span>
 
<span class="token keyword">function</span> <span class="token generic-function"><span class="token function">canRollOver</span><span class="token generic class-name"><span class="token operator">&lt;</span><span class="token constant">T</span> <span class="token keyword">extends</span> Constructor<span class="token operator">&gt;</span></span></span><span class="token punctuation">(</span>Animal<span class="token operator">:</span> <span class="token constant">T</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">return</span> <span class="token keyword">class</span> <span class="token class-name"><span class="token keyword">extends</span></span> Animal <span class="token punctuation">{</span>
    <span class="token function">rollOver</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      <span class="token builtin">console</span><span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">"rolled over"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
 
<span class="token keyword">const</span> TrainedDog <span class="token operator">=</span> <span class="token function">canRollOver</span><span class="token punctuation">(</span>Dog<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> rover <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">TrainedDog</span><span class="token punctuation">(</span><span class="token string">"Rover"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
 
rover<span class="token punctuation">.</span><span class="token function">rollOver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  <span class="token comment">// valid</span>
rover<span class="token punctuation">.</span><span class="token function">rollsOver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Error: Property 'rollsOver' does not exist on type ...</span></code></pre>



<p>The type of <code>rover</code> will be <code>Dog &amp; (mixin class)</code>, which is effectively Dog with a <code>rollOver</code> method.</p>



<h3>Enums</h3>



<p>TypeScript includes an <a href="https://www.typescriptlang.org/docs/handbook/enums.html#enums" target="_blank" rel="noreferrer noopener nofollow">enum</a> type that allows for the efficient representation of sets of constant values. For example, from the TypeScript specification, an enumeration of possible styles to apply to text might look like this:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">enum</span> Style <span class="token punctuation">{</span>
  <span class="token constant">NONE</span> <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">,</span>
  <span class="token constant">BOLD</span> <span class="token operator">=</span> <span class="token number">1</span><span class="token punctuation">,</span>
  <span class="token constant">ITALIC</span> <span class="token operator">=</span> <span class="token number">2</span><span class="token punctuation">,</span>
  <span class="token constant">UNDERLINE</span> <span class="token operator">=</span> <span class="token number">4</span><span class="token punctuation">,</span>
  <span class="token constant">EMPHASIS</span> <span class="token operator">=</span> Style<span class="token punctuation">.</span><span class="token constant">BOLD</span> <span class="token operator">|</span> Style<span class="token punctuation">.</span><span class="token constant">ITALIC</span><span class="token punctuation">,</span>
  <span class="token constant">HYPERLINK</span> <span class="token operator">=</span> Style<span class="token punctuation">.</span><span class="token constant">BOLD</span> <span class="token operator">|</span> Style<span class="token punctuation">.</span><span class="token constant">UNDERLINE</span>
<span class="token punctuation">}</span></code></pre>



<p>Enums can be initialized with constants or via computed values, or they can be auto-initialized, or a mix of initializations. Note that auto-initialized entries must come before entries are initialized with computed values.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">enum</span> Directions <span class="token punctuation">{</span>
  North<span class="token punctuation">,</span> <span class="token comment">// will have value 0</span>
  South<span class="token punctuation">,</span> <span class="token comment">// will have value 1</span>
  East <span class="token operator">=</span> <span class="token function">getDirectionValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
  West <span class="token operator">=</span> <span class="token number">10</span>
<span class="token punctuation">}</span></code></pre>



<p>Enum values can also be strings or a mix of numbers and other literals.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">enum</span> Color <span class="token punctuation">{</span>
  Red <span class="token operator">=</span> <span class="token string">"RED"</span><span class="token punctuation">,</span>
  Green <span class="token operator">=</span> <span class="token string">"GREEN"</span><span class="token punctuation">,</span>
  Blue <span class="token operator">=</span> <span class="token string">"BLUE"</span>
<span class="token punctuation">}</span></code></pre>



<p>Numeric enums are two-way maps, so you can determine the name of an enumerated value by looking it up in the enum object. For example, using the <code>Style</code> above example, <code>Style[1]</code> would evaluate to ‘BOLD’. Literal-initialized enums cannot be reverse mapped.</p>



<p>In earlier versions of TypeScript, numeric and literal enums had a few other subtle differences that could cause confusion. Numeric enum values effectively had the same type as the containing enum itself but were just numbers in any other context, which allowed for their values to be computed, and for instances of the enum to be initialized from numeric values rather than member names. Members of literal enums could not be computed, and each had their own distinct type, with the containing enum type being a union of all its member types. TypeScript 5.0 unified these two enum variations meaning all enum types are unions of their member types, and computed member values are allowed in enum literals.</p>



<p>Enums are real objects, not just typing constructs, so they exist at runtime, and incur some runtime cost. That isn’t normally a problem, but for cases where constraints are tight, <code>const enum</code> may help. When <code>const</code> is applied to <code>enum</code>, the compiler will replace all uses of the enum with literal values at compile time, so that no runtime cost is incurred. Note that all entries in a <code>const enum</code> must be auto-initialized or be initialized with constant expressions (no computed values).</p>



<h3>Ambient declarations</h3>



<p>Statically typed code is great, but there are still some libraries that don’t include typings. TypeScript can work with these out of the box, but without the full benefit of typed code. Luckily, TypeScript also has a mechanism for adding types to legacy and/or external code: <a href="https://www.typescriptlang.org/docs/handbook/modules.html#working-with-other-javascript-libraries" target="_blank" rel="noreferrer noopener nofollow">ambient declarations</a>.</p>



<p>Ambient declarations describe the types, or “shape”, of existing code, but don’t provide an implementation. Various constructs, such as variables, classes, and functions, can be declared using the keyword <code>declare</code>. For example, the global variable installed by jQuery is defined in the jQuery typings on <a href="https://github.com/DefinitelyTyped/DefinitelyTyped" target="_blank" rel="noreferrer noopener nofollow">DefinitelyTyped</a> (a public repository of third party typings for JavaScript packages) as:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">declare</span> <span class="token keyword">const</span> jQuery<span class="token operator">:</span> JQueryStatic<span class="token punctuation">;</span>
<span class="token keyword">declare</span> <span class="token keyword">const</span> $<span class="token operator">:</span> JQueryStatic<span class="token punctuation">;</span></code></pre>



<p>When these typings are included in a project, TypeScript will understand that there are <code>jQuery</code> and <code>$</code> global variables with the type <code>JQueryStatic</code>.</p>



<p>One of the most common use cases for ambient types is to provide typings for entire modules or packages. For example, assume we have a “vetUtils” package that exports some classes useful for veterinary applications, like Pet and Dog. An ambient module declaration for the vetUtils module would look like this:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">declare</span> <span class="token keyword">module</span> <span class="token string">"vetUtils"</span> <span class="token punctuation">{</span>
  <span class="token keyword">export</span> <span class="token keyword">class</span> <span class="token class-name">Pet</span> <span class="token punctuation">{</span>
    id<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
    name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
    <span class="token function">constructor</span><span class="token punctuation">(</span>id<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">,</span> name<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token keyword">export</span> <span class="token keyword">class</span> <span class="token class-name">Dog</span> <span class="token keyword">extends</span> <span class="token class-name">Pet</span> <span class="token punctuation">{</span>
    <span class="token function">bark</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token keyword">void</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre>



<p>Assuming the declaration above was in a file <code>vetUtils.d.ts</code> that was included in a project, TypeScript would use the typings in the ambient declaration whenever a module imported resources from “vetUtils”. Note the <code>d.ts</code> extension. This is the extension for a <a href="https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html" target="_blank" rel="noreferrer noopener">declaration file</a>, which can only contain types, no actual code. Since these files only contain type declarations, TypeScript does not generate compiled code for them.</p>



<p>For ambient declarations to be useful, TypeScript needs to know about them. There are two ways to explicitly let the TS compiler know about declaration files. One is to include declaration files directly in the compilation with the <code>files</code> or <code>include</code> directives in the <code>tsconfig.json file</code>. The other is with a reference <a href="https://www.typescriptlang.org/docs/handbook/triple-slash-directives.html#-reference-path-" target="_blank" rel="noreferrer noopener nofollow">triple-slash directive</a> at the top of a source file:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token comment">/// &lt;reference types="jquery" /&gt;</span>
<span class="token comment">/// &lt;reference path="../types/vetUtils" /&gt;</span></code></pre>



<p>These comments tell the compiler that a declaration file needs to be loaded. The <code>types</code> form looks for types in packages, similar to how module importing works. The <code>path</code> form gives a path to a declaration file. In both cases, the compiler will identify the directives during preprocessing and add the declaration files to the compilation.</p>



<p>The TS compiler will also look for type declarations in specific locations. By default, it will load ambient types in any package under <code>node_modules/@types</code>. So, for example, if a project includes the <code>@types/node</code> package, the compiler will have type definitions for standard Node modules such as <code>fs</code> and <code>path</code>, as well as for global values like <code>process</code>.</p>



<p>The set of directories TS looks to for types may be configured with the <a href="https://www.typescriptlang.org/tsconfig#typeRoots" target="_blank" rel="noreferrer noopener">typeRoots</a> compiler option. A similar <a href="https://www.typescriptlang.org/tsconfig#types" target="_blank" rel="noreferrer noopener">types</a> option can be used to specify which types of packages are loaded. In both cases, the options will replace the default behavior. If typeRoots is specified, node_modules/@types will not be included unless it’s listed in typeRoots. Similarly, if types were set to [“node”], only the node typings would be automatically loaded, even if more types were available in <code>node_modules/@types</code> (or whatever directories were in <code>typeRoots</code>).</p>



<h3>Loader plugins</h3>



<p>If you’re an AMD user, you’ll probably be used to working with <a href="https://github.com/amdjs/amdjs-api/wiki/Loader-Plugins" target="_blank" rel="noreferrer noopener nofollow">loader plugins</a> (<code>text!</code> and the like). TypeScript doesn’t understand plugin style module identifiers, and although it can emit AMD code with this type of module ID, it can’t load and parse the referenced modules for type checking purposes, at least not without some help. Originally, that meant <code>amd-dependency</code> triple-slash directives:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token comment">/// &lt;amd-dependency path="text!foo.html" name="foo" /&gt;</span>

<span class="token keyword">declare</span> <span class="token keyword">const</span> foo<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">;</span>
<span class="token builtin">console</span><span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>foo<span class="token punctuation">)</span><span class="token punctuation">;</span></code></pre>



<p>This directive tells TypeScript that it should add a <code>text!foo.html</code> dependency to the emitted AMD code, and that the name for the loaded dependency should be “foo”.</p>



<p>Since TypeScript 2, though, the preferred way to handle AMD dependencies is with <a href="https://www.typescriptlang.org/docs/handbook/modules.html#wildcard-module-declarations" target="_blank" rel="noreferrer noopener nofollow">wildcard modules</a> and imports. In a <code>.d.ts</code> file, a wildcard module declaration describes how all imports through the plugin behave. For the <code>text</code>, plugin, an import will result in a string:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">declare</span> <span class="token keyword">module</span> <span class="token string">"text!*"</span> <span class="token punctuation">{</span>
  <span class="token keyword">let</span> text<span class="token operator">:</span> text<span class="token punctuation">;</span>
  <span class="token keyword">export</span> <span class="token keyword">default</span> text<span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre>



<p>Any files that need to use the plugin can then use standard import statements:</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">import</span> foo <span class="token keyword">from</span> <span class="token string">"text!foo.html"</span><span class="token punctuation">;</span></code></pre>



<h3>JSX support</h3>



<p>TypeScript started becoming popular not long after React, and it gained support for React’s <a href="https://www.typescriptlang.org/docs/handbook/jsx.html" target="_blank" rel="noreferrer noopener nofollow">JSX syntax</a> (including the ability to type check it) in version 1.6. To use JSX syntax in TypeScript, code must be in a file with a <code>.tsx</code> extension, &nbsp;and the <a href="https://www.typescriptlang.org/tsconfig#jsx" target="_blank" rel="noreferrer noopener">jsx</a> compiler option must be enabled.</p>



<p>TypeScript is a compiler, and by default it transforms JSX to standard JS using the <code>React.createElement</code> and <code>React.Fragment APIs</code>. For interoperability in different build scenarios, it can also emit JSX in <code>.jsx</code> files, or JSX in <code>.js</code> files, configurable with the <code>jsx</code> option. The factory and fragment functions can also be changed with the <a href="https://www.typescriptlang.org/tsconfig#jsxFactory" target="_blank" rel="noreferrer noopener nofollow">jsxFactory</a> and <a href="https://www.typescriptlang.org/tsconfig#jsxFragmentFactory" target="_blank" rel="noreferrer noopener nofollow">jsxFragmentFactory</a> options.</p>



<h3>Control flow analysis</h3>



<p>TypeScript performs control flow analysis to catch common errors and other issues that can lead to maintenance headaches, including (but not limited to):</p>



<ul>
<li>unreachable code</li>



<li>unused labels</li>



<li>implicit returns</li>



<li>case clause fall-throughs</li>



<li>strict null checking</li>
</ul>



<p>While having the compiler catch this type of issue can be very helpful, it can be a problem when adding TS to legacy projects. Many of the issues TS can catch don’t cause code to fail, but can make it harder to understand and maintain, and existing JS code may have many instances of them. Developers may not want to deal with these issues all at once, so the TS compiler allows these checks to be individually disabled with <a href="https://www.typescriptlang.org/tsconfig" target="_blank" rel="noreferrer noopener">compiler flags</a> such as <code>&lt;a href="https://www.typescriptlang.org/tsconfig#allowUnreachableCode" target="_blank" rel="noreferrer noopener"&gt;allowUnreachableCode&lt;/a&gt;</code> and <code>&lt;a href="https://www.typescriptlang.org/tsconfig#noFallthroughCasesInSwitch" target="_blank" rel="noreferrer noopener"&gt;noFallthroughCasesInSwitch&lt;/a&gt;</code>.</p>



<h3>Compiler comments</h3>



<p>To make migrating legacy code easier, some special comments can be used to control how TS analyzes specific files or parts of files:</p>



<ul>
<li><code>// @ts-nocheck</code> – A file with this comment at the top won’t be type checked</li>



<li><code>// @ts-check</code> – When the checkJs compiler option isn’t set, .js files will be processed by the compiler but not type checked. Adding this comment to the top of a .js file will cause it to be type checked.</li>



<li><code>// @ts-ignore</code> – Suppress any type checking errors for the following line of code</li>



<li><code>// @ts-expect-error</code> – Suppress a type checking error for the following line of code. Raise a compilation error if the following line <em>doesn’t</em> have a type checking error.</li>
</ul>



<p>The <code>@ts-check</code> and <code>@ts-nocheck</code> comments historically only applied to <code>.js</code> files, but as of TS 3.7, <code>@ts-nocheck</code> can also be used for <code>.ts</code> files.</p>



<p>The <code>@ts-expect-error</code> comment is new in TS 3.9. It is useful in situations where a developer needs to intentionally use an invalid type, such as in unit tests. For example, a test that validates some runtime behavior may need to call a function with an invalid value. Using the <code>@ts-expect-error</code> comment, the test can call the function with invalid data without generating a compiler warning, <em>and</em> the compiler will also verify that the function’s input is properly typed.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token comment">// src/util.ts</span>

<span class="token keyword">function</span> <span class="token function">checkValue</span><span class="token punctuation">(</span>val<span class="token operator">:</span> <span class="token builtin">string</span><span class="token punctuation">)</span><span class="token operator">:</span> <span class="token builtin">boolean</span> <span class="token punctuation">{</span>
  <span class="token comment">// ...</span>
<span class="token punctuation">}</span></code></pre>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token comment">// tests/unit/util.ts</span>

<span class="token function">test</span><span class="token punctuation">(</span><span class="token string">'checkName with invalid data'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token comment">// @ts-expect-error</span>
  assert<span class="token punctuation">.</span><span class="token function">isFalse</span><span class="token punctuation">(</span><span class="token function">checkValue</span><span class="token punctuation">(</span><span class="token number">5</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span></code></pre>



<p>The <code>@ts-ignore</code> comment could also be used to suppress the error in the example above. However, using <code>@ts-expect-error</code> lets the compiler alert the developer if the argument types to <code>checkValue</code> change. For example, if <code>checkValue</code> was updated to accept <code>string | number</code>, the compiler would emit an error for the test code because <code>checkValue(5)</code> no longer caused the expected type error. That would be actionable information since <code>checkValue(5)</code> was no longer properly testing the invalid data case.</p>



<h3>Types in try/catch statements</h3>



<p>By default, values captured in a catch statement are defined as an <code>any</code> type.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">try</span> <span class="token punctuation">{</span>
   <span class="token keyword">throw</span> <span class="token string">"error"</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token punctuation">{</span>
   <span class="token comment">// err is "any" type</span>
<span class="token punctuation">}</span></code></pre>



<p>As of TypeScript 4, you can declare these values as unknown types, since that type fits better than <code>any</code>.</p>



<pre class="wp-block-prismatic-blocks language-typescript" tabindex="0"><code class="language-typescript"><span class="token keyword">try</span> <span class="token punctuation">{</span>
   <span class="token keyword">throw</span> <span class="token string">"error"</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span>err<span class="token operator">:</span> <span class="token builtin">unknown</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
   <span class="token comment">// err is "unknown" type</span>
<span class="token punctuation">}</span></code></pre>



<p>In TypeScript 4.4, you can make the values captured in catch statements be defined as <code>unknown</code> by default by enabling the <code>useUnknownInCatchVariables</code> configuration option. This option is enabled by default when using the <code>strict</code> option.</p>