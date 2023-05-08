Rev.B05.5について<br>
<br>
１．ファームウェアの変更<br>
開発者、奥江 聡（@S_Okue）さんのファームウェア変更に対応しました。<br>
詳細はこちら→https://twitter.com/S_Okue/status/1648836575680417792<br>
<br>
２．GAME80の変更
<br>
元々GAME80は、EMUZ80上で動作するようにし、SuperMEZ80でも動作させましたが、<br>
CP/M-80 Ver2.2用で動作するように修正、拡張したため、<br>
SuperMEZ80上でのGAME80の使い勝手と、仕様に違いが出ていました。<br>
<br>
今回、CP/M-80 Ver2.2用のGAME80IC Ver.03をEMUZ80、SuperMEZ80で動かすように<br>
しました。使い勝手と、仕様が同じになります。<br>
GAME80ICとは、インタープリタとコンパイラを一つにまとめ、コンパイラの起動に未使用の<br>
変数を割り当てたものです。詳細は、<br>
https://github.com/akih-san/GAME-and-VTL-for-CP-M
をご覧ください。<br>
<br>
GAME80ICを起動すると、<br>
<br>
プログラムロードバージョンでは、<br>
「GAME80IC Ver.03 for CP/M-2.2 and SuperMZ80」と表示されます。<br>
<br>
プログラム全部入りバージョンでは。
「GAME80IC Ver.03 for AKI-80 and SuperMZ80」と表示されます。<br>
<br>
<br>
<br>
それ以外は変わっていません。<br>
<br>
モニターの表示は、「Rev.B05」と表示されます。<br>
<br>
<br>
#######################################################<br>
########## SuperMEZ80-10MHz ###########################<br>
#######################################################<br>
<br><br>
SuperMEZ80にEMUZ80モニタを移植しました。<br>
ターゲットCPUはzilog Z84C0010PEG 10MHzのCMOS版Z80です。定格の10MHzで使用しています。<br>
ROMプログラム容量の関係でPIC18F47Q84のみサポートしています。<br>
PICのファームウェアに、UARTのXON/XOFFフロー制御を実装し、115.2Kbpsの通信ができます。<br>
CPUが10MHz以下のものを使用する場合は、ファームのボーレートを調整する必要があります。<br>

// UART3 initialize<br>
//	U3BRG = 416;	// 9600bps @ 64MHz<br>
//	U3BRG = 208;	// 19200bps @ 64MHz<br>
//	U3BRG = 104;	// 38400bps @ 64MHz
U3BRG = 34;		// 115200bps @ 64MHz<br>
<br>
サンプルボーレートが幾つか選べますので、トライアンドエラーを行ってみてください。
<br>
TeraTermでの接続例です。<br>
https://user-images.githubusercontent.com/112370246/213708107-5d3125b2-3d94-4d20-b701-4bde53d1b619.png)
<br>
<br>
2通りのプログラムを公開します。<br>
<br>
<br>
１．プログラムロードバージョン（メモリの空き容量重視のプログラム）<br>
<br>
モニタープログラムのみＰＩＣに書き込まれています。起動時に、メモリの後方（Ｄ０００Ｈ）にモニターは転送され、<br>
40番地からフリーエリアとなります。<br>
<br>
（一般的には100Hからなんでしょうけど、割り込み無いので、40Hからフリーとした）<br>
<br>
まぁ、シャープのMZ-80クリーンコンピュータ・・・みたいな（笑）<br>
BASIC等のプログラムは、モニタのロード機能を使います。115Kで通信しますので、ロードも早いです。<br>
<br>
書き込むファイルは、SuperMZ80_LOAD_RevB055フォルダ内のSuperMEZ_B055.X.production.hexです。<br>
ロードするプログラムは、programフォルダにあります。<br>
<br>
PICのファームウェアソースは、emuz80_z80ram.cです。<br>
<br>
<br>
<br>
２．プログラム全部入りバージョン（EMUZ80と同じ形式）<br>
<br>
EMUZ80と同様にPICに、GBASIC、TINY BASIC、GAME80、GAME80コンパイラ、VTLZ80を収納してあります。<br>
フリーエリアは３２Ｋとなります。３２Ｋで十分だという方は、こちらを使用してください。<br>
書き込むファイルは、SuperMEZ80FULL_RevB05フォルダ内のSuperMEZ_FULL_B055.X.production.hex<br>
です。<br>
<br>
PICのファームウェアソースは、emuz80_z80ram_all.cです。<br>
<br>
<br>
<br>
((((( モニタの機能拡張 )))))<br>
<br>
モニタRev.B04から、LGコマンド（Load and Go）の機能を追加しました。<br>
ヘキサファイルをロード後、ロードした先頭アドレスにジャンプします。<br>
<br>
<br>
<br>
（メモリマップ）<br>
<br>
[SuperMEZ80メモリマップ.pdf](https://github.com/akih-san/SuperMEZ80/blob/main/SuperMEZ80%E3%83%A1%E3%83%A2%E3%83%AA%E3%83%9E%E3%83%83%E3%83%97.pdf)<br>
<br>
<br>
（サンプルプログラム）<br>
<br>
・startrek_classic<br>
      懐かしいaltair basic star trekをGBASICで動かせるようにしてみました。<br>
      元のソースは、( http://www.bobsoremweb.com/startrek.html )で公開されています。<br>
<br>
・GAME80用のライフゲーム<br>
・GAME80用のオセロゲーム<br>
<br>
<br>
（モニターのプログラムソース）<br>
<br>
EMUZ80-MON Rev.B05 ( https://github.com/akih-san/EMUZ80-MON )で公開していますので、<br>
<br>
そちらを参照してください。<br>
<br>
