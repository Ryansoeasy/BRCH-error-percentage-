<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>百分比誤差計算機</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 0; padding: 20px;
      background-color: #f2f2f2;
      background-image: url('https://upload.wikimedia.org/wikipedia/commons/thumb/e/e2/BASF-Logo_bw.svg/512px-BASF-Logo_bw.svg.png');
      background-repeat: no-repeat; background-position: center bottom;
      background-size: 50%; background-attachment: fixed;
    }
    .container { max-width: 480px; margin: auto; background: white;
      padding: 20px; border-radius: 10px; box-shadow: 0 0 8px rgba(0,0,0,.1);
    }
    h2 { text-align: center; font-size: 22px; margin-bottom: 20px; }
    label { display: block; margin-top: 10px; font-size: 16px; }
    input, button { width: 100%; padding: 10px; margin-top: 5px;
      font-size: 16px; box-sizing: border-box;
    }
    button { background-color: #0070ba; color: white; border: none;
      border-radius: 5px; margin-top: 20px;
    }
    #result { margin-top: 20px; font-size: 18px; text-align: center;
      color: #333;
    }
    .history-item { border-top: 1px dashed #ccc; padding-top: 8px;
      margin-top: 8px; font-size: 15px;
    }
    h3 { margin-top: 30px; font-size: 18px; }
  </style>
</head>
<body>
  <div class="container">
    <h2>百分比誤差計算機</h2>
    <label>實際吐出量 (g/min)：</label>
    <input type="number" id="actual" placeholder="請輸入實際值">
    <label>設計吐出量 (g/min)：</label>
    <input type="number" id="design" placeholder="請輸入設計值">
    <button onclick="calculate()">計算</button>
    <div id="result"></div>
    <h3>歷史紀錄（最多 5 筆）</h3>
    <div id="history"></div>
  </div>
  <script>
    const historyList = [];
    function calculate() {
      const actual = parseFloat(document.getElementById('actual').value);
      const design = parseFloat(document.getElementById('design').value);
      const resultDiv = document.getElementById('result');
      const historyDiv = document.getElementById('history');
      if (isNaN(actual) || isNaN(design) || actual === 0) {
        resultDiv.innerHTML = '⚠️ 請輸入有效數值，且實際值不能為 0。';
        return;
      }
      const percentError = Math.abs((design - actual) / actual) * 100;
      resultDiv.innerHTML = `百分比誤差為：<strong>${percentError.toFixed(2)}%</strong>`;
      historyList.unshift({ actual, design, percentError: percentError.toFixed(2) });
      if (historyList.length > 5) historyList.pop();
      historyDiv.innerHTML = '';
      historyList.forEach((item, idx) => {
        historyDiv.innerHTML += `
          <div class="history-item">
            #${idx + 1} 實際：${item.actual}，設計：${item.design}，
            百分比誤差：${item.percentError}%
          </div>`;
      });
    }
  </script>
</body>
</html>
