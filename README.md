# gen-music-for-airports
gen-music-for-airports

## About

このプロジェクトは、ブライアン・イーノの「Music for Airports 2/1」をWeb Audio API（Tone.js）を使用して再現したものです。以下、主要な構成要素と機能について詳しく説明します。

📁 プロジェクト構成
index.html - アプリケーション全体を含む単一のHTMLファイル
README.md - プロジェクトの概要とGitHub ActionsとClaude Codeの統合についての説明
LICENSE - MITライセンスファイル
🎵 技術的な実装
1. 音声生成システム (index.html:367-506)
7つの異なる音声ループを使用して、非同期のアンビエント音楽を生成します：

const loopData = [
    { note: "G#4", length: 17.8, volume: -18, synthType: 'fatsine', ... },
    { note: "C4",  length: 20.1, volume: -19, synthType: 'fatsine', ... },
    // ... 他5つのループ
];
各ループは以下の要素を持ちます：

音程（note）: G#4、C4、C#4、F4、D#4、G#3、F3
ループ長（length）: 16.2秒〜31.8秒の異なる長さ
音量（volume）: -17dB〜-20dB
音色設定: エンベロープ（Attack/Decay/Sustain/Release）と倍音構成
2. バイオフィードバック機能 (index.html:509-622)
心拍数（HR）と心拍変動（HRV）のデータを使用して、音楽パラメータをリアルタイムで調整：

function applyBioFeedback(hr, hrv) {
    const tempoScale = clamp(hr/60, 0.6, 1.5);  // テンポの調整
    const relax = 1/(1+Math.exp(-(hrv-30)/15));  // リラックス度の計算
    // ループ長、音量、LFO、エンベロープなどを動的に調整
}
3. シンセサイザー設定 (index.html:667-729)
Tone.jsを使用して各ループのシンセサイザーを作成：

const synth = new Tone.Synth({
    oscillator: {
        type: data.synthType,
        count: 4,
        spread: 15
    },
    envelope: {
        attack: data.envelope.attack,
        decay: data.envelope.decay,
        sustain: data.envelope.sustain,
        release: data.envelope.release
    },
    volume: data.volume
}).connect(globalReverb);
4. 空間音響処理 (index.html:671-677)
長いディケイタイムのリバーブを使用して空間的な響きを作成：

globalReverb = new Tone.Reverb({
    decay: 15,  // 15秒のディケイタイム
    wet: parseFloat(reverbWetSlider.value),
    preDelay: 0.05
}).connect(masterVolume);
5. LFO（低周波発振器） (index.html:701-710)
アナログの揺らぎをエミュレートするため、各シンセにLFOを適用：

const pitchLFO = new Tone.LFO({
    frequency: 0.02,
    min: -3,
    max: 3,
    amplitude: 1
}).start();
pitchLFO.connect(synth.detune);
6. ビジュアルエフェクト (index.html:1066-1154)
p5.jsを使用した3Dパーティクルアニメーション：

5000個のパーティクルが音量に応じて動的に変化
3D空間での回転と移動
音声レベルに基づいて半径が変化
🎛️ ユーザーインターフェース
コントロールパネル
バイオフィードバック

HR（心拍数）: 40-120 bpm
HRV（心拍変動）: 10-120 ms RMSSD
音響コントロール

マスター音量
リバーブ量
各シンセサイザーの詳細設定

ループ長の調整
音量コントロール
倍音構成（4つの倍音）
エンベロープ（ADSR）
LFO設定（周波数、最小値、最大値）
🔄 ループ管理システム
各ループは独立して動作し、プログレスバーでリアルタイムの再生位置を表示。ユーザーはプログレスバーをクリックして再生位置を変更可能。

📱 レスポンシブデザイン
モバイル対応のCSSメディアクエリを使用（index.html:126-143）

主な特徴
非同期ループ: 7つのループが異なる長さで独立して動作
リアルタイム制御: 再生中でもパラメータを調整可能
視覚的フィードバック: プログレスバーとパーティクルアニメーション
バイオフィードバック統合: 生体データに基づく音楽の自動調整
このプロジェクトは、実験的な電子音楽とWeb Audio技術を組み合わせた、インタラクティブなアンビエント音楽生成システムです。

Readme

### License

MIT license
