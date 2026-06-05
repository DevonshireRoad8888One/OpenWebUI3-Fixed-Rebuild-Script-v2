# OpenWebUI3-Fixed-Rebuild-Script-v2
OpenWebUI3 Fixed Rebuild Script v2 Auto installer for Ubuntu.
Just paste this script into the Ubuntu home folder terminal, and it will install Open Web.UI.
Then enter this command to start the server, cd ~/openwebui3 && ./start.sh
The first time you run this it will install everything, then when you start for the first time you will be on the log in page, and you will make your account and password. Once that is done the next time you use the start command, cd ~/openwebui3 && ./start.sh you will come to the login page, enter your email & password.
That's it, you now have Open Web.UI installed.
It is as simple as all that, Thanks for trying my program.
gpt-5.5
Yes — this error makes sense.

The problem was not Open WebUI this time. The problem was this part:

text

Collapse
Save
Copy
1
2
shell-init: error retrieving current directory: getcwd: cannot access parent directories
FileNotFoundError: [Errno 2] No such file or directory
That happened because your terminal was still located inside:

bash

Collapse
Save
Copy
1
~/openwebui3
Then the script deleted that same folder:

bash

Collapse
Save
Copy
1
rm -rf "$HOME/openwebui3"
So the terminal was “standing inside a folder that no longer exists.” Then pip failed because it could not find the current working directory.

The fix is simple: the rebuild script must first move safely back to your home folder:

bash

Collapse
Save
Copy
1
cd "$HOME"
before deleting ~/openwebui3.

Below is the corrected OpenWebUI3 rebuild script v2.

OpenWebUI3 Fixed Rebuild Script v2
Copy/paste this whole block:

bash

Collapse
Save
Copy
300
301
302
303
304
305
306
307
308
309
310
311
312
313
314
315
316
317
318
319
320
321
322
323
324
325
326
327
328
329
330
331
332
333
334
335
336
337
338
339
340
341
342
343
344
345
346
347
348
349
350
351
352
353
354
355
356
357
358
cd "$HOME"

echo "✅ OpenWebUI3 stopped."
STOP3

chmod +x "$BASE/start.sh" "$BASE/stop.sh"

echo "📝 Creating COMMANDS.txt..."

cat > "$BASE/COMMANDS.txt" <<COMMANDS3
Open WebUI Test Bench #3 Commands
=================================

Start:
cd ~/openwebui3 && ./start.sh

Stop:
cd ~/openwebui3 && ./stop.sh

Open in browser:
http://127.0.0.1:8001

Check WebUI #3:
curl -s http://127.0.0.1:8001/api/version

Check isolated Ollama #3:
curl -s http://127.0.0.1:11436/api/version

Pull model into isolated Ollama #3:
OLLAMA_HOST=127.0.0.1:11436 ollama pull llama3.2:3b

List isolated Ollama #3 models:
OLLAMA_HOST=127.0.0.1:11436 ollama list

View Ollama #3 log:
tail -50 ~/openwebui3/ollama.log

Live Ollama #3 log:
tail -f ~/openwebui3/ollama.log

Important fixed start line:
exec "\$BASE/env1/bin/open-webui" serve --host 127.0.0.1 --port "\$WEBUI_PORT"
COMMANDS3

echo "=================================================="
echo "✅ Open WebUI Test Bench #3 rebuilt successfully."
echo "📁 Folder: $BASE"
echo "🌐 WebUI:  http://127.0.0.1:$WEBUI_PORT"
echo "🧠 Ollama: http://127.0.0.1:$OLLAMA_PORT"
echo "=================================================="

echo
echo "🚀 Starting OpenWebUI3 now..."
cd "$BASE"
./start.sh
REBUILD3

chmod +x "$HOME/rebuild_openwebui3_v2.sh"

"$HOME/rebuild_openwebui3_v2.sh"
What Changed in v2
The important new safety fixes are:

bash

Collapse
Save
Copy
1
cd "$HOME"
before deleting the old folder:

bash

Collapse
Save
Copy
1
rm -rf "$BASE"
And this before using pip:

bash

Collapse
Save
Copy
1
cd "$BASE"
So pip always has a real working folder.

The important Open WebUI port fix is still included:

bash

Collapse
Save
Copy
1
exec "$BASE/env1/bin/open-webui" serve --host 127.0.0.1 --port "$WEBUI_PORT"
Expected Result
This time it should rebuild cleanly, then start on:

text

Collapse
Save
Copy
1
http://127.0.0.1:8001
And it should use isolated Ollama on:

text

Collapse
Save
Copy
1
http://127.0.0.1:11436
If it starts successfully, open:

text

Collapse
Save
Copy
1
http://127.0.0.1:8001
You should get the first-login page again.

Hello the, OpenWebUI3 Fixed Rebuild Script v2, has worked flawlessly…

profile
gpt-5.5
Today at 9:38 PM
Excellent, my friend — that is exactly what we wanted. 🎉

That means OpenWebUI3 Fixed Rebuild Script v2 is now a confirmed working test-bench installer.

We have verified these important fixes:

✅ What Worked
1. Folder rebuild works correctly
The script safely does:

bash

Collapse
Save
Copy
1
2
cd "$HOME"
rm -rf "$HOME/openwebui3"
So it no longer deletes the folder while the terminal is standing inside it.

That fixed the getcwd problem:

text

Collapse
Save
Copy
1
2
getcwd: cannot access parent directories
FileNotFoundError: [Errno 2] No such file or directory
2. WebUI port fix works
The important fixed line works:

bash

Collapse
Save
Copy
1
exec "$BASE/env1/bin/open-webui" serve --host 127.0.0.1 --port "$WEBUI_PORT"
So Open WebUI #3 correctly starts on:

text

Collapse
Save
Copy
1
http://127.0.0.1:8001
Instead of trying to use the default:

text

Collapse
Save
Copy
1
http://127.0.0.1:8080
3. Isolated Ollama works
OpenWebUI3 uses isolated Ollama on:

text

Collapse
Save
Copy
1
http://127.0.0.1:11436
With models stored in:

text

Collapse
Save
Copy
1
~/openwebui3/models
4. Isolated data folder works
OpenWebUI3 stores its WebUI data in:

text

Collapse
Save
Copy
1
~/openwebui3/data
So it is separate from:

text

Collapse
Save
Copy
1
2
~/openwebui
~/openwebui2
Current Confirmed System Layout
System
Folder
WebUI Port
Ollama Port
Main OpenWebUI
~/openwebui
8080
11434
OpenWebUI2 Test
~/openwebui2
8000
11435
OpenWebUI3 Test
~/openwebui3
8001
11436


This is a very good clean test layout.

Keep These OpenWebUI3 Commands
Start #3
bash

Collapse
Save
Copy
1
cd ~/openwebui3 && ./start.sh
Stop #3
bash

Collapse
Save
Copy
1
cd ~/openwebui3 && ./stop.sh
Check #3 WebUI
bash

Collapse
Save
Copy
1
curl -s http://127.0.0.1:8001/api/version
Check #3 Ollama
bash

Collapse
Save
Copy
1
curl -s http://127.0.0.1:11436/api/version
Pull model into #3 Ollama
bash

Collapse
Save
Copy
1
OLLAMA_HOST=127.0.0.1:11436 ollama pull llama3.2:3b
List #3 models
bash

Collapse
Save
Copy
1
OLLAMA_HOST=127.0.0.1:11436 ollama list
Important Notes for the GitHub Version
The two big fixes we should keep in the final GitHub autoinstaller are:

Fix #1 — Always leave the folder before deleting it
Before deleting an install folder, always do:

bash

Collapse
Save
Copy
1
cd "$HOME"
Then delete:

bash

Collapse
Save
Copy
1
rm -rf "$BASE"
This prevents the broken-current-directory error.

Fix #2 — Force Open WebUI to the selected port
Do not rely only on environment variables.

Use:

bash

Collapse
Save
Copy
1
exec "$BASE/env1/bin/open-webui" serve --host 127.0.0.1 --port "$WEBUI_PORT"
Instead of:

bash

Collapse
Save
Copy
1
exec open-webui serve
That is the fix that stopped Open WebUI from defaulting back to 8080.

Status
✅ openwebui3 rebuild script confirmed working
✅ Port 8001 confirmed working
✅ Ollama port 11436 confirmed working
✅ Folder-delete bug fixed
✅ Ready to use as the template for the final autoinstaller

Very nice progress. This is now a reliable working pattern we can build the GitHub version from.
