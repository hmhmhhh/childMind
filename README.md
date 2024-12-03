# Healthy Brain Network (HBN) データセットについて

HBNデータセットは、5～22歳の約5,000人を対象にした臨床および研究のスクリーニングデータです。この研究の目的は、客観的な生物学的視点から精神的健康や学習障害の診断および治療を改善するための生物学的マーカーを見つけることです。

今回のコンペティションでは、この研究の以下の2つのデータが使用されています：
1. **身体活動データ**（手首装着型加速度計データ、フィットネス評価、質問票）
2. **インターネット使用行動データ**

目標は、これらのデータを用いて、インターネット依存の標準的な指標である「Severity Impairment Index（sii）」を予測することです。

## コンペティションの概要
- **形式**：コードコンペティション（テストセットは非公開）
- **提供データ**：サンプルデータが正しい形式で公開されており、フルテストセットは約3,800例。
- **データ形式**：加速度計データはParquetファイル、その他のデータはCSVファイル。
- **課題**：多くの測定値が欠損しており、トレーニングデータ内でも一部の参加者のsii値が欠けている。非教師あり学習の適用が考えられる。

## データの構成

### 1. Tabularデータ（train.csv/test.csv）
各参加者の情報を含む。主な項目は以下の通り：
- **Demographics**：年齢と性別。
- **Internet Use**：1日のインターネット使用時間。
- **Children's Global Assessment Scale**：18歳未満の一般的な機能を評価する数値スケール。
- **Physical Measures**：血圧、心拍数、身長、体重、腰・臀部のサイズ。
- **FitnessGram Vitals and Treadmill**：NHANESトレッドミルプロトコルを用いた心肺機能評価。
- **FitnessGram Child**：5つのパラメータを測定する体力評価。
- **Bio-electric Impedance Analysis**：BMI、体脂肪、筋肉量、水分量などの体組成。
- **Physical Activity Questionnaire**：過去7日間の活動に関する情報。
- **Sleep Disturbance Scale**：睡眠障害を分類するスケール。
- **Actigraphy**：加速度計を用いた身体活動の客観的測定。
- **Parent-Child Internet Addiction Test (PCIAT)**：インターネット依存を評価する20項目スケール。「PCIAT_Total」フィールドは、このコンペの目標siiを基にした値（0～3）。

### 2. 加速度計データ（Parquet形式）
参加者が最大30日間、日常生活中に装着した加速度計データを含む。各データには以下が含まれる：
- **id**：参加者の識別子。
- **step**：タイムステップ。
- **X, Y, Z**：各軸の加速度測定値（g単位）。
- **enmo**：加速度信号の「Euclidean Norm Minus One」値。
- **anglez**：腕の角度（水平面基準）。
- **non-wear_flag**：装着していない期間のフラグ。
- **light**：周囲の光量（lux）。
- **battery_voltage**：バッテリ電圧（mV）。
- **time_of_day**：データの時間帯（5秒間隔）。
- **weekday**：曜日（1:月曜日～7:日曜日）。
- **quarter**：四半期（1～4）。
- **relative_date_PCIAT**：PCIATテストからの日数（負の値はテスト前に収集）。

### 3. sample_submission.csv
提出形式の例が記載されたサンプルファイル。

## sii予測のターゲット
ターゲットフィールドは、インターネット使用行動を示す「PCIAT_Total」から導出された値で、以下のように分類されます：
- **0**：None
- **1**：Mild
- **2**：Moderate
- **3**：Severe
