<div class="Box-body">
        <article class="markdown-body entry-content p-5" itemprop="text"><h1><a id="user-content-edittextalphabet" class="anchor" aria-hidden="true" href="#edittextalphabet"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>EditTextAlphabet</h1>
<p>If you don't want to allow users to enter digits and special character into edittext using mobile keyboard then you can use this code.</p>
<h2><a id="user-content-how-to-make-edittext-accepts-only-alphabetno-digits-or-special-characters" class="anchor" aria-hidden="true" href="#how-to-make-edittext-accepts-only-alphabetno-digits-or-special-characters"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>How to make EditText accepts only Alphabet(No digits or Special Characters)?</h2>
<p>I made <strong>AlphabetsSymbolsInputFilter</strong> class which you can use with your EditText. Even you can specify special characters which you want to allow from user.</p>
<pre><code>class AlphabetsSymbolsInputFilter(symbols: String) : InputFilter {

        private var mWordPattern: String
        var mLetterPattern: String

        init {
            mLetterPattern = "[a-zA-Z.$symbols ]"
            //mLetterPattern = "[a-zA-Z0-9.$symbols ]" // replace if alphanumeric
            mWordPattern = "$mLetterPattern+"
        }

        override fun filter(
            source: CharSequence,
            start: Int,
            end: Int,
            dest: Spanned,
            dstart: Int,
            dend: Int
        ): CharSequence? {
            if (source == "") {
                println("In backspace")
                return source
            }
            if (source.isNotEmpty() &amp;&amp; source.toString().matches(mWordPattern.toRegex())) {
                return source
            }
            var sourceStr = ""
            if (source.isNotEmpty() &amp;&amp; !source.toString().matches(mLetterPattern.toRegex())) {
                sourceStr = source.toString()
                while (sourceStr.isNotEmpty() &amp;&amp; !sourceStr.matches(mWordPattern.toRegex())) {
                    println(" source --&gt; $source dest ---&gt; $dest")
                    if (sourceStr.last().isDigit()) {
                        print("Is digit ")
                        sourceStr = sourceStr.subSequence(0, sourceStr.length - 1).toString()
                    } else if (!sourceStr.last().toString().matches(mLetterPattern.toRegex())) {
                        print("Is emoji or weird symbols")
                        sourceStr = sourceStr.subSequence(0, sourceStr.length - 1).toString()
                    } else
                        break
                }
                return sourceStr
            }
            return source
        }
    }

</code></pre>
<p>Use below function with EditText filter.</p>
<pre><code>  fun editTextAllowAlphabetsSymbols(symbols: String): Array&lt;InputFilter&gt; {
        return arrayOf(AlphabetsSymbolsInputFilter(symbols))
    }
</code></pre>
<h2><a id="user-content-how-to-use-alphabetssymbolsinputfilter-with-edittext" class="anchor" aria-hidden="true" href="#how-to-use-alphabetssymbolsinputfilter-with-edittext"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>How to use AlphabetsSymbolsInputFilter with EditText?</h2>
<p>Just need to call <strong>editTextAllowAlphabetsSymbols()</strong> function with EditText's Filter Property.</p>
<pre><code>  editTextName.filters = editTextAllowAlphabetsSymbols("")
</code></pre>
<p>If you want to allow user to use special character like '$' than you can do like below.</p>
<pre><code> editTextName.filters = editTextAllowAlphabetsSymbols("$")
</code></pre>
<h2><a id="user-content-where-to-use-alphabetssymbolsinputfilter-class" class="anchor" aria-hidden="true" href="#where-to-use-alphabetssymbolsinputfilter-class"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Where to use AlphabetsSymbolsInputFilter class?</h2>
<p>You can use when taking user input for</p>
<ul>
<li>Name, First Name and Last Name</li>
<li>Birthplace</li>
<li>State
or any places when you don't want to allow user to add digit and special characters.</li>
</ul>
</article>
      </div>
  </div>
