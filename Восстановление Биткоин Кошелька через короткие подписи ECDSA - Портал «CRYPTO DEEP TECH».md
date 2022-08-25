<p>Всем нам известно, что раскрываемость секретного ключа в подписи ECDSA может привести к полному восстановлению Биткоин Кошелька. В наших более ранних статьях мы рассматривали <a href="https://cryptodeep.ru/lattice-attack/" target="_blank" rel="noreferrer noopener">слабости и уязвимости</a> в транзакциях блокчейна, но так же существуют короткие подписи ECDSA которые так же приводят к полному восстановлению Биткоин Кошелька.</p>



<h3>Почему же эти подписи ECDSA называются короткими?</h3>



<blockquote class="wp-block-quote"><p>Ответ на этот вопрос вы можете получить из обсуждаемой темы: <a href="https://bitcoin.stackexchange.com/questions/38513/the-shortest-ecdsa-signature" target="_blank" rel="noreferrer noopener"><strong>«Самая короткая подпись ECDSA» [The shortest ECDSA signature]</strong></a></p></blockquote>



<p>В прошлой нашей статье: <a href="https://cryptodeep.ru/reduce-private-key/" target="_blank" rel="noreferrer noopener"><em>«Уменьшение приватного ключа через скалярное умножение используем библиотеку ECPy + Google Colab»</em></a>мы создали <em>Python</em>-скрипт: <a href="https://github.com/demining/CryptoDeepTools/blob/main/08ReducePrivateKey/maxwell.py" target="_blank" rel="noreferrer noopener">maxwell.py</a> который сгенерировал для нас довольно интересный публичный ключ</p>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/69a7a0b324ac3b1f3e0dc27f0f4130bf.png" alt="Восстановление Биткоин Кошелька через короткие подписи ECDSA"></figure>



<pre class="wp-block-code"><code>(0x3b78ce563f89a0ed9414f5aa28ad0d96d6795f9c63 , 0xc0c686408d517dfd67c2367651380d00d126e4229631fd03f8ff35eef1a61e3c)</code></pre>



<blockquote class="wp-block-quote"><p>Как мы знаем значение сигнатуры <code>"R"</code> это и есть публичный ключ от секретного ключа <code>(Nonce)</code></p></blockquote>



<p>Взгляните на Blockchain транзакцию: <a href="https://btc.exan.tech/tx/11e6b169701a9047f3ddbb9bc4d4ab1a148c430ba4a5929764e97e76031f4ee3" target="_blank" rel="noreferrer noopener"><strong>11e6b169701a9047f3ddbb9bc4d4ab1a148c430ba4a5929764e97e76031f4ee3</strong></a></p>



<h3>RawTX:</h3>



<pre class="wp-block-code"><code>0100000001afddd5c9f05bd937b24a761606581c0cddd6696e05a25871279f75b7f6cf891f250000005f3c303902153b78ce563f89a0ed9414f5aa28ad0d96d6795f9c6302200a963d693c008f0f8016cfc7861c7f5d8c4e11e11725f8be747bb77d8755f1b8012103151033d660dc0ef657f379065cab49932ce4fb626d92e50d4194e026328af853ffffffff010000000000000000016a00000000
</code></pre>



<blockquote class="wp-block-quote"><p>Размер этой транзакции всего лишь: <code>156 байт</code></p></blockquote>



<h3>Как можно восстановить Биткоин Кошелек через короткие подписи ECDSA?</h3>



<p>В криптоанализе блокчейна криптовалюты Bitcoin мы используем собственный <em>Bas</em>h-скрипт: <code>btcrecover.sh</code></p>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/299fa2616cbdb8e6df9304862f441ba2.gif" alt="Процесс восстановление Биткоин Кошелька" title="Процесс восстановление Биткоин Кошелька"><figcaption>Процесс восстановление Биткоин Кошелька</figcaption></figure>



<h3>Bash-скрипт: btcrecover.sh</h3>



<pre class="wp-block-code"><code>pip2 install -r requirements.txt
chmod +x btcrecover.sh


 ./btcrecover.sh 12yysAMhagEm67QCX85p3WQnTUrqcvYVuk


 ./btcrecover.sh 15HvLBX9auG2bJdLCTxSvjvWvdgsW7BvAT
</code></pre>



<h3>Результаты:</h3>



<p><code>| privkey : addr |</code></p>



<p>Откроем&nbsp;<a href="https://cryptodeep.ru/bitaddress.html" target="_blank" rel="noreferrer noopener">bitaddress</a>&nbsp;и проверим:</p>



<pre class="wp-block-code"><code>ac8d0abda1d32aaabff56cb72bc39a998a98779632d7fee83ff452a86a849bc1:12yysAMhagEm67QCX85p3WQnTUrqcvYVuk
b6c1238de89e9defea3ea0712e08726e338928ac657c3409ebb93d9a0873797f:15HvLBX9auG2bJdLCTxSvjvWvdgsW7BvAT</code></pre>



<h2>Перейдем к экспериментальной части и более детально разберем все скрипты по восстановлению Биткоин Кошелька</h2>



<p>Откроем&nbsp;<a href="https://github.com/demining/TerminalGoogleColab" target="_blank" rel="noreferrer noopener"><strong>[TerminalGoogleColab]</strong></a>.</p>



<p>Воспользуемся репозиторием <a href="https://github.com/demining/CryptoDeepTools/tree/main/09BitcoinWalletRecovery" target="_blank" rel="noreferrer noopener"><strong>«09BitcoinWalletRecovery»</strong></a>.</p>



<pre class="wp-block-code"><code>git clone https://github.com/demining/CryptoDeepTools.git

cd CryptoDeepTools/09BitcoinWalletRecovery/

ls</code></pre>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/4e37b8f6cefc8f8d0553f541a5eb8b98.png" alt="Восстановление Биткоин Кошелька через короткие подписи ECDSA"></figure>



<h3>Установим все нужные модули:</h3>



<pre class="wp-block-code"><code>bitcoin
ecdsa
utils
base58</code></pre>



<hr class="wp-block-separator has-alpha-channel-opacity">



<pre class="wp-block-code"><code>pip2 install -r requirements.txt</code></pre>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/d5f25e5b1b966ff43a48aaf0955ecfcd.png" alt="Восстановление Биткоин Кошелька через короткие подписи ECDSA"></figure>



<blockquote class="wp-block-quote"><p>С помощью скрипта <a href="https://github.com/demining/CryptoDeepTools/blob/main/09BitcoinWalletRecovery/breakECDSA.py" target="_blank" rel="noreferrer noopener">breakECDSA.py</a> мы получим из <code>RawTX</code> сигнатуры [R, S, Z]</p></blockquote>



<pre class="wp-block-code"><code>python2 breakECDSA.py 0100000001afddd5c9f05bd937b24a761606581c0cddd6696e05a25871279f75b7f6cf891f250000005f3c303902153b78ce563f89a0ed9414f5aa28ad0d96d6795f9c6302200a963d693c008f0f8016cfc7861c7f5d8c4e11e11725f8be747bb77d8755f1b8012103151033d660dc0ef657f379065cab49932ce4fb626d92e50d4194e026328af853ffffffff010000000000000000016a00000000 &gt; signatures.txt
</code></pre>



<h3>Результат будет сохранен в файл: signatures.txt</h3>



<p>Откроем файл: <code>PublicKeys.txt</code></p>



<pre class="wp-block-code"><code>cat signatures.txt</code></pre>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/cd32b99e37cc600ddd3c35965e2a0288.png" alt="Восстановление Биткоин Кошелька через короткие подписи ECDSA"></figure>



<pre class="wp-block-code"><code>R = 0x00000000000000000000003b78ce563f89a0ed9414f5aa28ad0d96d6795f9c63
S = 0x0a963d693c008f0f8016cfc7861c7f5d8c4e11e11725f8be747bb77d8755f1b8
Z = 0x521a65420faa5386d91b8afcfab68defa02283240b25aeee958b20b36ddcb6de</code></pre>



<p>Как нам известно из прошлой нашей <a href="https://habr.com/ru/post/682220/">статьи</a>, нам известен <em><u>секретный ключ</u></em> к генерации <em>сигнатуры</em> <code>R</code></p>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/9dfb1763d978473245abb6cab03e0e48.png" alt="Восстановление Биткоин Кошелька через короткие подписи ECDSA"></figure>



<blockquote class="wp-block-quote"><p>В нашем случае <em><u>секретный ключ</u></em> <code>(Nonce)</code>:</p></blockquote>



<pre class="wp-block-code"><code>0x7fffffffffffffffffffffffffffffff5d576e7357a4501ddfe92f46681b20a0 --&gt; 0x3b78ce563f89a0ed9414f5aa28ad0d96d6795f9c63, 0x3f3979bf72ae8202983dc989aec7f2ff2ed91bdd69ce02fc0700ca100e59ddf3
</code></pre>



<h3>Signatures:</h3>



<pre class="wp-block-code"><code>K = 0x7fffffffffffffffffffffffffffffff5d576e7357a4501ddfe92f46681b20a0
R = 0x00000000000000000000003b78ce563f89a0ed9414f5aa28ad0d96d6795f9c63
S = 0x0a963d693c008f0f8016cfc7861c7f5d8c4e11e11725f8be747bb77d8755f1b8
Z = 0x521a65420faa5386d91b8afcfab68defa02283240b25aeee958b20b36ddcb6de</code></pre>



<blockquote class="wp-block-quote"><p>Теперь когда нам известны значение <code>[K, R, S, Z</code>] мы можем получить приватный ключ по формуле и восстановить Биткоин Кошелек.</p></blockquote>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/2dba14ab5cb76228181c68a9403038a7.svg" alt="Privkey = ((((S * K) - Z) * modinv(R,N)) % N)"></figure>



<p>Для получения приватного ключа воспользуемся <em>Python</em>-скриптом: <a href="https://github.com/demining/CryptoDeepTools/blob/main/09BitcoinWalletRecovery/calculate.py" target="_blank" rel="noreferrer noopener">calculate.py</a></p>



<pre class="wp-block-code"><code>def h(n):
    return hex(n).replace("0x","")

def extended_gcd(aa, bb):
    lastremainder, remainder = abs(aa), abs(bb)
    x, lastx, y, lasty = 0, 1, 1, 0
    while remainder:
        lastremainder, (quotient, remainder) = remainder, divmod(lastremainder, remainder)
        x, lastx = lastx - quotient*x, x
        y, lasty = lasty - quotient*y, y
    return lastremainder, lastx * (-1 if aa &lt; 0 else 1), lasty * (-1 if bb &lt; 0 else 1)

def modinv(a, m):
    g, x, y = extended_gcd(a, m)
    if g != 1:
        raise ValueError
    return x % m
    
N = 0xfffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd0364141


K = 0x7fffffffffffffffffffffffffffffff5d576e7357a4501ddfe92f46681b20a0
R = 0x00000000000000000000003b78ce563f89a0ed9414f5aa28ad0d96d6795f9c63
S = 0x0a963d693c008f0f8016cfc7861c7f5d8c4e11e11725f8be747bb77d8755f1b8
Z = 0x521a65420faa5386d91b8afcfab68defa02283240b25aeee958b20b36ddcb6de


print (h((((S * K) - Z) * modinv(R,N)) % N))</code></pre>



<h3>Запустим Python-скрипт: calculate.py</h3>



<pre class="wp-block-code"><code>python3 calculate.py</code></pre>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/502645324ec5666803f791ea3cc3a109.png" alt="PrivKey = b6c1238de89e9defea3ea0712e08726e338928ac657c3409ebb93d9a0873797f" title="PrivKey = b6c1238de89e9defea3ea0712e08726e338928ac657c3409ebb93d9a0873797f"><figcaption>PrivKey = b6c1238de89e9defea3ea0712e08726e338928ac657c3409ebb93d9a0873797f</figcaption></figure>



<p class="has-medium-font-size">Откроем&nbsp;<a href="https://cryptodeep.ru/bitaddress.html" target="_blank" rel="noreferrer noopener">bitaddress</a>&nbsp;и проверим:</p>



<pre class="wp-block-code"><code>ADDR: 15HvLBX9auG2bJdLCTxSvjvWvdgsW7BvAT
WIF:  L3LxjEnwKQMFYNYmCGzM1TqnwxRDi8UyRzQpVfmDvk96fYN44oFG
HEX:  b6c1238de89e9defea3ea0712e08726e338928ac657c3409ebb93d9a0873797f</code></pre>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/82b5e115789ac3949bbe28bb975a2826.png" alt="Восстановление Биткоин Кошелька через короткие подписи ECDSA"></figure>



<p class="has-medium-font-size"><strong>Приватный ключ найден!</strong></p>



<p class="has-medium-font-size"><strong>Биткоин кошелек восстановлен!</strong></p>



<figure class="wp-block-image"><img src="./Восстановление Биткоин Кошелька через короткие подписи ECDSA - Портал «CRYPTO DEEP TECH»_files/50fe20dbc6d9d6f4a4dbf43c6eaba268.png" alt="Восстановление Биткоин Кошелька через короткие подписи ECDSA"></figure>



<blockquote class="wp-block-quote"><p><code>Короткие подписи ECDSA</code> — <em>это потенциальная угроза потери монет</em> <code>BTC</code>, <em>поэтому мы настоятельно рекомендуем всем всегда обновлять ПО и использовать только проверенные устройства.</em></p></blockquote>



<p>Данный видеоматериал создан для портала&nbsp;<a href="https://cryptodeep.ru/" target="_blank" rel="noreferrer noopener"><strong>CRYPTO DEEP TECH</strong></a>&nbsp;для обеспечения финансовой безопасности данных и криптографии на эллиптических кривых&nbsp;<code>secp256k1</code>&nbsp;против слабых подписей&nbsp;<code>ECDSA</code>&nbsp;в криптовалюте&nbsp;<code>BITCOIN</code></p>



<p><a href="https://github.com/demining/CryptoDeepTools/tree/main/09BitcoinWalletRecovery" target="_blank" rel="noreferrer noopener"><strong>Исходный код</strong></a></p>



<p><a href="https://t.me/cryptodeeptech"><strong>Telegram</strong></a><strong>:&nbsp;</strong><a href="https://t.me/cryptodeeptech" target="_blank" rel="noreferrer noopener"><strong><u>https://t.me/cryptodeeptech</u></strong></a></p>



<p><strong><a href="https://youtu.be/xBgjWE5tA7Y" target="_blank" rel="noreferrer noopener">Видеоматериал:&nbsp;https://youtu.be/xBgjWE5tA7Y</a></strong></p>



<p><a href="https://cryptodeep.ru/shortest-ecdsa-signature" target="_blank" rel="noreferrer noopener"><strong>Источник: https://cryptodeep.ru/shortest-ecdsa-signature</strong></a></p>
	</div><!-- .entry-content -->

	
