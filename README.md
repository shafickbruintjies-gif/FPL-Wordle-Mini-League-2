<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>FPL Wordle League</title>
</head>
<body style="margin:0;padding:0;background:#0d0d1a;color:#e8e8f8;font-family:Georgia,serif;min-height:100vh;">

<div style="background:#38003c;padding:20px;text-align:center;">
  <div style="font-size:28px;font-weight:900;color:#fff;">FPL Wordle</div>
  <div style="font-size:28px;font-weight:900;color:#00ff85;">Football Fanatics</div>
  <div style="font-size:10px;letter-spacing:3px;color:#a070b0;margin-top:6px;font-family:monospace;">ONE TWO · /10 SCORING</div>
</div>

<div style="display:flex;border-top:2px solid #1a0020;">
  <button id="btn-table"  onclick="show('table')"  style="flex:1;padding:14px 4px;background:#00ff85;color:#38003c;border:none;border-right:1px solid #1a0020;font-family:monospace;font-size:11px;font-weight:900;letter-spacing:2px;cursor:pointer;">🏆 TABLE</button>
  <button id="btn-submit" onclick="show('submit')" style="flex:1;padding:14px 4px;background:#38003c;color:#7a4a8a;border:none;border-right:1px solid #1a0020;font-family:monospace;font-size:11px;font-weight:900;letter-spacing:2px;cursor:pointer;">⚽ SUBMIT</button>
  <button id="btn-manage" onclick="show('manage')" style="flex:1;padding:14px 4px;background:#38003c;color:#7a4a8a;border:none;font-family:monospace;font-size:11px;font-weight:900;letter-spacing:2px;cursor:pointer;">👥 SQUAD</button>
</div>

<!-- Loading indicator -->
<div id="loading" style="padding:40px;text-align:center;font-family:monospace;font-size:13px;color:#5a3a6a;letter-spacing:2px;">LOADING...</div>

<!-- TABLE -->
<div id="view-table" style="padding:16px;display:none;">
  <div style="display:grid;grid-template-columns:36px 1fr 38px 44px 48px;gap:4px;padding:6px 10px;margin-bottom:4px;">
    <div style="font-size:9px;letter-spacing:2px;color:#5a3a6a;font-family:monospace;font-weight:700;"></div>
    <div style="font-size:9px;letter-spacing:2px;color:#5a3a6a;font-family:monospace;font-weight:700;">PLAYER</div>
    <div style="font-size:9px;letter-spacing:2px;color:#5a3a6a;font-family:monospace;font-weight:700;text-align:center;">SUB</div>
    <div style="font-size:9px;letter-spacing:2px;color:#5a3a6a;font-family:monospace;font-weight:700;text-align:center;">AVG</div>
    <div style="font-size:9px;letter-spacing:2px;color:#5a3a6a;font-family:monospace;font-weight:700;text-align:center;">PTS</div>
  </div>
  <div id="table-body"></div>
  <div style="margin-top:16px;padding:12px;background:rgba(56,0,60,0.4);border-radius:6px;font-family:monospace;font-size:11px;color:#8060a0;line-height:1.8;">
    1/10=10pts · 2=9 · 3=8 · 4=7 · 5=6 · 6=5 · 7=4 · 8=3 · 9=2 · 10=1 · X=0
  </div>
</div>

<!-- SUBMIT -->
<div id="view-submit" style="padding:16px;display:none;">
  <div style="margin-bottom:14px;">
    <div style="font-size:10px;letter-spacing:3px;color:#00ff85;font-family:monospace;margin-bottom:8px;">PLAYER</div>
    <select id="player-select" style="width:100%;padding:12px;background:#10001a;border:1px solid #3a1a4a;color:#e8e8f8;border-radius:6px;font-size:15px;font-family:Georgia,serif;"></select>
  </div>
  <div style="margin-bottom:14px;">
    <div style="font-size:10px;letter-spacing:3px;color:#00ff85;font-family:monospace;margin-bottom:8px;">PASTE SHARE TEXT</div>
    <textarea id="paste-area" rows="7" style="width:100%;padding:12px;background:#10001a;border:1px solid #3a1a4a;color:#e8e8f8;border-radius:6px;font-size:13px;font-family:monospace;resize:vertical;line-height:1.7;" placeholder="One Two FPL Wordle May 21&#10;7/10&#10;&#10;🟥🟩🟥..."></textarea>
  </div>
  <div id="submit-msg" style="display:none;padding:12px;border-radius:6px;font-family:monospace;font-size:13px;margin-bottom:14px;"></div>
  <button onclick="submitScore()" style="width:100%;padding:15px;background:#00ff85;color:#38003c;border:none;border-radius:6px;font-size:14px;font-weight:900;font-family:monospace;letter-spacing:2px;cursor:pointer;">⚽ SUBMIT SCORE</button>
</div>

<!-- SQUAD -->
<div id="view-manage" style="padding:16px;display:none;">
  <div style="margin-bottom:14px;">
    <div style="font-size:10px;letter-spacing:3px;color:#00ff85;font-family:monospace;margin-bottom:8px;">ADD PLAYER</div>
    <div style="display:flex;gap:8px;">
      <input id="new-name" type="text" placeholder="Player name" style="flex:1;padding:12px;background:#10001a;border:1px solid #3a1a4a;color:#e8e8f8;border-radius:6px;font-size:15px;font-family:Georgia,serif;outline:none;"/>
      <button onclick="addPlayer()" style="padding:12px 18px;background:#00ff85;color:#38003c;border:none;border-radius:6px;font-family:monospace;font-weight:900;font-size:13px;cursor:pointer;">ADD</button>
    </div>
    <div id="add-msg" style="display:none;margin-top:8px;padding:10px;border-radius:6px;font-family:monospace;font-size:12px;"></div>
  </div>
  <div id="squad-list" style="background:rgba(56,0,60,0.35);border:1px solid #2a0a3a;border-radius:8px;overflow:hidden;margin-bottom:14px;"></div>
  <button onclick="resetAll()" style="padding:10px 18px;background:transparent;border:1px solid #4a1a2a;color:#e63946;border-radius:4px;font-family:monospace;font-size:12px;cursor:pointer;">Reset All Scores</button>
</div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/12.13.0/firebase-app.js";
import { getDatabase, ref, set, onValue } from "https://www.gstatic.com/firebasejs/12.13.0/firebase-database.js";

const firebaseConfig = {
  apiKey: "AIzaSyAwP7iGnQ8yXV-Kj81KlMud1WO-TBUvM00",
  authDomain: "wordle-league-2.firebaseapp.com",
  databaseURL: "https://wordle-league-2-default-rtdb.firebaseio.com",
  projectId: "wordle-league-2",
  storageBucket: "wordle-league-2.firebasestorage.app",
  messagingSenderId: "144707072220",
  appId: "1:144707072220:web:5f1bc44ad8799c77d78419",
  measurementId: "G-92KNB55M73"
};

const app = initializeApp(firebaseConfig);
const db  = getDatabase(app);

var players  = [];
var scores   = {};
var expanded = {};
var POINTS   = {1:10,2:9,3:8,4:7,5:6,6:5,7:4,8:3,9:2,10:1,'X':0};
var ready    = false;

onValue(ref(db, 'league'), function(snapshot) {
  var data = snapshot.val() || {};
  players  = data.players || [];
  scores   = data.scores  || {};
  if (!ready) {
    ready = true;
    document.getElementById('loading').style.display = 'none';
    show('table');
  } else {
    renderTable();
    renderSelect();
    renderSquad();
  }
});

function save() {
  set(ref(db, 'league'), { players: players, scores: scores });
}

window.show = function(v) {
  ['table','submit','manage'].forEach(function(s) {
    document.getElementById('view-'+s).style.display = v===s ? 'block' : 'none';
    document.getElementById('btn-'+s).style.background = v===s ? '#00ff85' : '#38003c';
    document.getElementById('btn-'+s).style.color      = v===s ? '#38003c' : '#7a4a8a';
  });
  if (v==='table')  renderTable();
  if (v==='submit') renderSelect();
  if (v==='manage') renderSquad();
};

window.toggleHistory = function(name) {
  expanded[name] = !expanded[name];
  renderTable();
};

window.submitScore = function() {
  document.getElementById('submit-msg').style.display = 'none';
  var player = document.getElementById('player-select').value;
  var text   = document.getElementById('paste-area').value.trim();
  if (!players.length || !player) { showMsg('submit-msg','⚠ Add players in SQUAD first.','#1a0010','#ff6b6b','1px solid #6a1a2a'); return; }
  if (!text) { showMsg('submit-msg','⚠ Paste your share text first.','#1a0010','#ff6b6b','1px solid #6a1a2a'); return; }
  var parsed = parseScore(text);
  if (!parsed) { showMsg('submit-msg',"⚠ Can't find a score like '7/10' in that text.",'#1a0010','#ff6b6b','1px solid #6a1a2a'); return; }
  var existing = scores[player] || [];
  if (parsed.puzzle) {
    for (var i=0; i<existing.length; i++) {
      if (existing[i].puzzle === parsed.puzzle) {
        showMsg('submit-msg','⚠ '+player+' already submitted for "'+parsed.puzzle+'".','#1a0010','#ff6b6b','1px solid #6a1a2a');
        return;
      }
    }
  }
  scores[player] = existing.concat([{ guesses:parsed.guesses, puzzle:parsed.puzzle, points:parsed.points, at:Date.now() }]);
  save();
  document.getElementById('paste-area').value = '';
  var emoji = parsed.guesses==='X'?'😬':parsed.guesses<=3?'🔥':parsed.guesses<=6?'✅':'😅';
  showMsg('submit-msg', emoji+' Saved! '+parsed.guesses+'/10 → '+parsed.points+' pts'+(parsed.puzzle?' ('+parsed.puzzle+')':''), 'rgba(0,255,133,0.06)','#00ff85','1px solid rgba(0,255,133,0.4)');
};

window.addPlayer = function() {
  var input = document.getElementById('new-name');
  var name  = input.value.trim();
  if (!name) { showMsg('add-msg','⚠ Enter a name.','#1a0010','#ff6b6b','1px solid #6a1a2a'); return; }
  if (players.indexOf(name) > -1) { showMsg('add-msg','⚠ Already exists.','#1a0010','#ff6b6b','1px solid #6a1a2a'); return; }
  players.push(name);
  save();
  input.value = '';
  document.getElementById('add-msg').style.display = 'none';
  renderSquad();
};

window.removePlayer = function(name) {
  players = players.filter(function(n){ return n!==name; });
  delete scores[name];
  delete expanded[name];
  save();
  renderSquad();
  renderTable();
};

window.resetAll = function() {
  if (!confirm('Reset ALL scores? This cannot be undone.')) return;
  scores = {};
  expanded = {};
  save();
  renderSquad();
  renderTable();
};

function parseScore(text) {
  var m = text.match(/([1-9]|10|X)\/\d+/i);
  if (!m) return null;
  var raw = m[1].toUpperCase();
  var g   = raw==='X' ? 'X' : parseInt(raw);
  var pm  = text.match(/(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)[a-z]* \d{1,2}/i);
  var pts = POINTS[g] !== undefined ? POINTS[g] : 0;
  return { guesses:g, puzzle: pm ? pm[0] : null, points:pts };
}

function renderTable() {
  var data = players.map(function(name) {
    var ps  = scores[name] || [];
    var pts = 0;
    for (var i=0; i<ps.length; i++) pts += ps[i].points||0;
    var avg = null;
    if (ps.length) {
      var s=0;
      for (var i=0; i<ps.length; i++) s += (ps[i].guesses==='X' ? 11 : ps[i].guesses);
      avg = (s/ps.length).toFixed(1);
    }
    return { name:name, pts:pts, played:ps.length, avg:avg, history:ps.slice().reverse() };
  });
  data.sort(function(a,b){ return b.pts-a.pts || (parseFloat(a.avg)||99)-(parseFloat(b.avg)||99); });

  var html = '';
  if (!data.length) {
    html = '<div style="padding:32px;text-align:center;color:#4a2a5a;font-family:monospace;font-size:13px;">No players yet — add some in SQUAD!</div>';
  } else {
    for (var i=0; i<data.length; i++) {
      var row    = data[i];
      var bg     = i===0 ? 'rgba(0,255,133,0.08)' : 'rgba(56,0,60,0.3)';
      var border = i===0 ? '1px solid rgba(0,255,133,0.3)' : '1px solid #1a0028';
      var medal  = i===0?'🥇':i===1?'🥈':i===2?'🥉':(i+1)+'';
      var ptsCol = i===0?'#00ff85':i===1?'#c0c0c0':i===2?'#cd7f32':'#e8e8f8';
      var sn     = row.name.replace(/\\/g,'\\\\').replace(/'/g,"\\'");
      var isOpen = expanded[row.name];

      html += '<div style="background:'+bg+';border:'+border+';border-radius:'+(isOpen?'6px 6px 0 0':'6px')+';margin-bottom:'+(isOpen?'0':'4px')+';overflow:hidden;">';
      html += '<div style="display:grid;grid-template-columns:36px 1fr 38px 44px 48px;gap:4px;align-items:center;padding:13px 10px;cursor:pointer;" onclick="toggleHistory(\''+sn+'\')">';
      html += '<div style="font-size:'+(i<3?'18':'13')+'px;text-align:center;">'+medal+'</div>';
      html += '<div style="padding-left:4px;"><div style="font-weight:700;font-size:14px;color:'+(i===0?'#fff':'#d0c0e0')+';">'+row.name+' <span style="font-size:10px;color:#5a3a6a;">'+(isOpen?'▲':'▼')+'</span></div>'
        + '<div style="font-size:10px;color:#5a3a6a;font-family:monospace;margin-top:1px;">tap for history</div></div>';
      html += '<div style="text-align:center;font-family:monospace;font-size:13px;font-weight:700;color:#a070c0;">'+row.played+'</div>';
      html += '<div style="text-align:center;font-family:monospace;font-size:12px;color:#8060a0;">'+(row.avg||'—')+'</div>';
      html += '<div style="text-align:center;font-family:monospace;font-size:18px;font-weight:900;color:'+ptsCol+';">'+row.pts+'</div>';
      html += '</div>';

      if (isOpen) {
        html += '<div style="background:#0d001a;border-top:1px solid #2a0a3a;padding:10px 12px;">';
        if (!row.history.length) {
          html += '<div style="font-family:monospace;font-size:12px;color:#4a2a5a;padding:6px 0;">No submissions yet.</div>';
        } else {
          html += '<div style="font-size:9px;letter-spacing:2px;color:#5a3a6a;font-family:monospace;margin-bottom:8px;">SUBMISSION HISTORY</div>';
          for (var j=0; j<row.history.length; j++) {
            var e  = row.history[j];
            var sc = e.guesses==='X'?'#e63946':e.guesses<=4?'#00ff85':e.guesses<=7?'#f4d03f':'#e67e22';
            var dt = e.puzzle || formatDate(e.at);
            html += '<div style="display:flex;justify-content:space-between;align-items:center;padding:6px 0;border-bottom:1px solid #1a0028;">'
              + '<div style="font-family:monospace;font-size:12px;color:#c0a0d0;">'+dt+'</div>'
              + '<div style="font-family:monospace;font-size:12px;"><span style="color:'+sc+';font-weight:700;">'+e.guesses+'/10</span>'
              + ' <span style="color:#f4d03f;margin-left:8px;">+'+e.points+'pts</span></div></div>';
          }
        }
        html += '</div>';
      }
      html += '</div>';
    }
  }
  document.getElementById('table-body').innerHTML = html;
}

function renderSelect() {
  var sel = document.getElementById('player-select');
  sel.innerHTML = players.length===0
    ? '<option>Add players in SQUAD first</option>'
    : players.map(function(p){ return '<option value="'+p+'">'+p+'</option>'; }).join('');
}

function renderSquad() {
  var html = '<div style="padding:12px 16px;border-bottom:1px solid #2a0a3a;font-size:10px;letter-spacing:3px;color:#00ff85;font-family:monospace;">SQUAD ('+players.length+')</div>';
  if (!players.length) {
    html += '<div style="padding:20px;text-align:center;color:#4a2a5a;font-family:monospace;font-size:13px;">No players yet</div>';
  } else {
    for (var i=0; i<players.length; i++) {
      var name = players[i];
      var ps   = scores[name]||[];
      var pts  = 0; for (var j=0; j<ps.length; j++) pts += ps[j].points||0;
      var sn   = name.replace(/\\/g,'\\\\').replace(/'/g,"\\'");
      html += '<div style="display:flex;align-items:center;justify-content:space-between;padding:12px 16px;border-bottom:'+(i<players.length-1?'1px solid #1a0028':'none')+';">'
        + '<div><div style="font-weight:700;font-size:14px;">'+name+'</div>'
        + '<div style="font-size:11px;color:#5a3a6a;font-family:monospace;">'+ps.length+' submission'+(ps.length!==1?'s':'')+' · '+pts+' pts</div></div>'
        + '<button onclick="removePlayer(\''+sn+'\')" style="padding:5px 12px;background:transparent;border:1px solid #4a1a2a;color:#e63946;border-radius:4px;font-family:monospace;font-size:10px;cursor:pointer;">RELEASE</button>'
        + '</div>';
    }
  }
  document.getElementById('squad-list').innerHTML = html;
}

function formatDate(ts) {
  if (!ts) return 'Unknown date';
  var d = new Date(ts);
  var months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  return months[d.getMonth()] + ' ' + d.getDate();
}

function showMsg(id, text, bg, color, border) {
  var el = document.getElementById(id);
  el.textContent = text;
  el.style.background = bg;
  el.style.color  = color;
  el.style.border = border;
  el.style.display = 'block';
}
</script>
</body>
</html>
