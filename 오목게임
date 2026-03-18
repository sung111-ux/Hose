import base64
from IPython.display import HTML, display

# 1. 특수 문자와 한글을 자바스크립트 내에서 동적으로 생성하여 파이썬 인코딩 오류 방지
game_html = """
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Linked List Omok</title>
    <style>
        body { background: #fdf6e3; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; font-family: sans-serif; }
        .container { background: white; padding: 30px; border-radius: 20px; box-shadow: 0 10px 40px rgba(0,0,0,0.1); text-align: center; }
        .board { display: grid; grid-template-columns: repeat(15, 35px); grid-template-rows: repeat(15, 35px); background: #deb887; padding: 10px; border: 3px solid #8b4513; margin: 20px auto; }
        .cell { width: 35px; height: 35px; border: 0.5px solid #8b4513; display: flex; justify-content: center; align-items: center; cursor: pointer; }
        .stone { width: 30px; height: 30px; border-radius: 50%; }
        .black { background: #000; box-shadow: 2px 2px 4px rgba(0,0,0,0.3); }
        .white { background: #fff; box-shadow: 2px 2px 4px rgba(0,0,0,0.2); border: 1px solid #ccc; }
        .status { font-size: 24px; font-weight: bold; margin-bottom: 15px; color: #333; }
        button { padding: 10px 20px; cursor: pointer; border-radius: 5px; border: none; background: #8b4513; color: white; font-weight: bold; }
    </style>
</head>
<body>
    <div class="container">
        <div class="status" id="status"></div>
        <div id="board" class="board"></div>
        <button id="reset-btn"></button>
    </div>
    <script>
        // 한글과 이모지를 자바스크립트 변수로 분리 (인코딩 오류 방지)
        const TXT = {
            blackTurn: "\\u26ab \\ud631\\ub3cc \\ucc28\\ub840", // ⚫ 흑돌 차례
            whiteTurn: "\\u26aa \\ubc31\\ub3cc \\ucc28\\ub840", // ⚪ 백돌 차례
            win: "\\ud83c\\udf8a ", // 🎊
            reset: "\\uac8c\\uc784 \\ucd08\\uae30\\ud654" // 게임 초기화
        };

        const SIZE = 15;
        let turn = 'black';
        let gameOver = false;
        let nodes = Array.from({length: SIZE}, () => Array(SIZE).fill(null));

        function init() {
            document.getElementById('status').innerText = TXT.blackTurn;
            document.getElementById('reset-btn').innerText = TXT.reset;
            document.getElementById('reset-btn').onclick = () => location.reload();

            const boardEl = document.getElementById('board');
            for(let r=0; r<SIZE; r++) {
                for(let c=0; c<SIZE; c++) {
                    const el = document.createElement('div');
                    el.className = 'cell';
                    el.onclick = () => play(r, c);
                    boardEl.appendChild(el);
                    nodes[r][c] = { r, c, color: null, el, next: {} };
                }
            }
            const dirs = {N:[-1,0], S:[1,0], E:[0,1], W:[0,-1], NE:[-1,1], NW:[-1,-1], SE:[1,1], SW:[1,-1]};
            for(let r=0; r<SIZE; r++) {
                for(let c=0; c<SIZE; c++) {
                    for(let d in dirs) {
                        let nr = r + dirs[d][0], nc = c + dirs[d][1];
                        if(nr>=0 && nr<SIZE && nc>=0 && nc<SIZE) nodes[r][c].next[d] = nodes[nr][nc];
                    }
                }
            }
        }

        function play(r, c) {
            if(nodes[r][c].color || gameOver) return;
            const node = nodes[r][c];
            node.color = turn;
            node.el.innerHTML = `<div class="stone ${turn}"></div>`;
            
            if(checkWin(node)) {
                const winner = turn === 'black' ? "\\ud631\\ub3cc" : "\\ubc31\\ub3cc";
                document.getElementById('status').innerText = TXT.win + winner + " \\uc2b9\\ub9ac!";
                gameOver = true;
                return;
            }
            turn = (turn === 'black') ? 'white' : 'black';
            document.getElementById('status').innerText = (turn === 'black' ? TXT.blackTurn : TXT.whiteTurn);
        }

        function checkWin(node) {
            const pairs = [['N','S'], ['E','W'], ['NE','SW'], ['NW','SE']];
            return pairs.some(p => {
                let cnt = 1;
                p.forEach(d => {
                    let curr = node.next[d];
                    while(curr && curr.color === node.color) { cnt++; curr = curr.next[d]; }
                });
                return cnt >= 5;
            });
        }
        init();
    </script>
</body>
</html>
"""

# 2. 파이썬에서 안전하게 Base64로 변환
# errors='ignore' 또는 'replace'를 써서 혹시 모를 대리쌍(surrogate) 오류 방지
b64_html = base64.b64encode(game_html.encode('utf-8', errors='replace')).decode('utf-8')

# 3. 출력 인터페이스
display(HTML(f'''
<div style="text-align:center; padding:40px; background:#fff; border:2px solid #8b4513; border-radius:15px; font-family:sans-serif;">
    <h2 style="color:#8b4513;">✅ 오목 게임 서버 패치 완료</h2>
    <p>인코딩 오류(UnicodeEncodeError)가 해결되었습니다.</p>
    <button onclick="openFinalGame()" style="padding:15px 30px; background:#8b4513; color:white; border:none; border-radius:8px; cursor:pointer; font-weight:bold; font-size:16px;">
        🎮 오목 게임 사이트 접속하기
    </button>
</div>

<script>
function openFinalGame() {{
    const b64 = "{b64_html}";
    const decoded = atob(b64);
    const win = window.open("", "_blank");
    win.document.open();
    win.document.write(decoded);
    win.document.close();
}}
</script>
'''))
