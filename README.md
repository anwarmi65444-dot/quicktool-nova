# quicktool-nova

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="Free online image and text tools. Resize, convert and compress images, count words, format JSON and more.">
<title>QuickTool Nova - Free Online Tools</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f3f6fb;color:#17233c;line-height:1.6}
header{background:#102344;color:white;text-align:center;padding:30px 15px}
header h1{margin:0;font-size:36px}
header p{margin:5px}
nav{display:flex;flex-wrap:wrap;justify-content:center;gap:15px;background:white;padding:15px}
nav a,footer a{color:#2457c5;text-decoration:none;font-weight:bold}
main{max-width:1100px;margin:25px auto;padding:0 15px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(min(100%,310px),1fr));gap:18px}
.card{background:white;padding:20px;border-radius:13px;border:1px solid #e0e6ef;box-shadow:0 4px 15px #17233c0a}
.card h2{margin:0 0 5px;font-size:22px}
.card p,.note{color:#5b6475;font-size:14px}
label{display:block;font-weight:bold;margin:10px 0 5px}
input[type=file]{max-width:100%;margin:10px 0}
input[type=number],select,textarea{width:100%;padding:11px;border:1px solid #ccd5e3;border-radius:7px;font:inherit}
textarea{min-height:115px;resize:vertical}
button{background:#2457c5;color:white;border:0;border-radius:7px;padding:11px 14px;margin:10px 5px 0 0;cursor:pointer;font-weight:bold}
button.secondary{background:#e8efff;color:#173f9d}
.output{margin-top:12px;overflow-wrap:anywhere;color:#176339;font-weight:bold}
footer{text-align:center;padding:25px}
</style>
</head>
<body>
<header>
<h1>QuickTool Nova</h1>
<p>Free, simple online tools for images, text and developers.</p>
</header>
<nav>
<a href="#images">Image Tools</a>
<a href="#words">Word Counter</a>
<a href="#case">Case Converter</a>
<a href="#spaces">Text Cleaner</a>
<a href="#json">JSON Formatter</a>
<a href="#base64">Base64 Tool</a>
</nav>
<main>
<h2>Free Online Tools</h2>
<p>Use these free tools directly in your browser.</p>
<div class="grid" id="images">

<section class="card">
<h2>Image Resizer</h2><p>Resize images to your desired dimensions.</p>
<input type="file" id="rf" accept="image/*">
<label for="rw">Width (pixels)</label><input type="number" id="rw" min="1" max="12000" value="500">
<label for="rh">Height (pixels)</label><input type="number" id="rh" min="1" max="12000" value="500">
<label><input type="checkbox" id="ratio" checked> Keep aspect ratio</label>
<button id="resize">Resize Image</button><div id="ro" class="output"></div>
</section>

<section class="card">
<h2>Image Format Converter</h2><p>Convert images to PNG, JPG or WebP.</p>
<input type="file" id="cf" accept="image/*">
<label for="fmt">Output format</label>
<select id="fmt"><option value="image/png">PNG</option><option value="image/jpeg">JPG</option><option value="image/webp">WebP</option></select>
<button id="convert">Convert Image</button><div id="co" class="output"></div>
</section>

<section class="card">
<h2>Image Compressor</h2><p>Reduce image file size using JPG compression.</p>
<input type="file" id="kf" accept="image/*">
<label for="quality">Quality</label><input type="range" id="quality" min="10" max="95" value="70">
<span id="qv">70%</span><br>
<button id="compress">Compress Image</button><div id="ko" class="output"></div>
</section>

<section class="card" id="words">
<h2>Word Counter</h2><p>Count words, characters and lines.</p>
<textarea id="wt" placeholder="Type or paste your text..."></textarea>
<div id="wo" class="output">Words: 0 · Characters: 0 · Lines: 0</div>
</section>

<section class="card" id="case">
<h2>Case Converter</h2><p>Change text capitalization.</p>
<textarea id="ct" placeholder="Enter text here..."></textarea>
<button data-case="upper">UPPERCASE</button>
<button data-case="lower">lowercase</button>
<button data-case="title">Title Case</button>
<button data-case="sentence" class="secondary">Sentence case</button>
<button id="ccopy" class="secondary">Copy Result</button>
<div id="cmsg" class="output"></div>
</section>

<section class="card" id="spaces">
<h2>Remove Extra Spaces</h2><p>Clean extra spaces and repeated blank lines.</p>
<textarea id="st" placeholder="Paste your text here..."></textarea>
<button id="clean">Clean Text</button>
<button id="scopy" class="secondary">Copy Result</button>
<label for="sr">Cleaned text</label><textarea id="sr" readonly></textarea>
<div id="smsg" class="output"></div>
</section>

<section class="card" id="json">
<h2>JSON Formatter</h2><p>Format, minify and validate JSON.</p>
<textarea id="jt" placeholder='{"name":"QuickTool Nova","free":true}'></textarea>
<button id="jf">Format JSON</button>
<button id="jm" class="secondary">Minify JSON</button>
<div id="jo" class="output"></div>
</section>

<section class="card" id="base64">
<h2>Base64 Encoder / Decoder</h2><p>Encode or decode text. Base64 is not encryption.</p>
<textarea id="bt" placeholder="Enter text or Base64..."></textarea>
<button id="be">Encode</button>
<button id="bd" class="secondary">Decode</button>
<div id="bo" class="output"></div>
</section>
</div>
</main>
<footer>
<p>© <span id="year"></span> QuickTool Nova</p>
<p class="note">Image processing happens in your browser. Your selected images are not uploaded to this website.</p>
<a href="#images">Back to top</a>
</footer>

<script>
const $=id=>document.getElementById(id);
$("year").textContent=new Date().getFullYear();

function getImage(id){
 const f=$(id).files[0];
 if(!f)throw Error("Please choose an image first.");
 if(!f.type.startsWith("image/"))throw Error("Please select a valid image.");
 return new Promise((resolve,reject)=>{
  const url=URL.createObjectURL(f),img=new Image();
  img.onload=()=>{URL.revokeObjectURL(url);resolve({img,file:f})};
  img.onerror=()=>{URL.revokeObjectURL(url);reject(Error("Could not open this image."))};
  img.src=url;
 });
}
function blobFromCanvas(canvas,type,quality){
 return new Promise((resolve,reject)=>canvas.toBlob(b=>b?resolve(b):reject(Error("Could not create this image format.")),type,quality));
}
function download(blob,name){
 const url=URL.createObjectURL(blob),a=document.createElement("a");
 a.href=url;a.download=name;document.body.appendChild(a);a.click();a.remove();
 setTimeout(()=>URL.revokeObjectURL(url),1500);
}
function baseName(file){return file.name.replace(/\.[^.]+$/,"").replace(/[^\w.-]+/g,"_")||"image"}

$("resize").onclick=async()=>{
 const out=$("ro");out.textContent="";
 try{
  const {img,file}=await getImage("rf");
  let w=Number($("rw").value),h=Number($("rh").value);
  if(!Number.isInteger(w)||!Number.isInteger(h)||w<1||h<1||w>12000||h>12000)throw Error("Enter dimensions between 1 and 12000.");
  if($("ratio").checked){
   const r=img.naturalWidth/img.naturalHeight;
   if(document.activeElement===$("rw"))h=Math.max(1,Math.round(w/r));
   else w=Math.max(1,Math.round(h*r));
  }
  const c=document.createElement("canvas");c.width=w;c.height=h;
  c.getContext("2d").drawImage(img,0,0,w,h);
  download(await blobFromCanvas(c,"image/png"),baseName(file)+"-resized.png");
  out.textContent=`Done: ${w} × ${h} pixels. Download started.`;
 }catch(e){out.textContent=e.message}
};

$("convert").onclick=async()=>{
 const out=$("co");out.textContent="";
 try{
  const {img,file}=await getImage("cf"),type=$("fmt").value;
  const c=document.createElement("canvas");c.width=img.naturalWidth;c.height=img.naturalHeight;
  const ctx=c.getContext("2d");
  if(type==="image/jpeg"){ctx.fillStyle="#fff";ctx.fillRect(0,0,c.width,c.height)}
  ctx.drawImage(img,0,0);
  const ext=type==="image/png"?"png":type==="image/jpeg"?"jpg":"webp";
  download(await blobFromCanvas(c,type,.92),baseName(file)+"."+ext);
  out.textContent="Converted successfully. Download started.";
 }catch(e){out.textContent=e.message}
};

$("quality").oninput=()=> $("qv").textContent=$("quality").value+"%";
$("compress").onclick=async()=>{
 const out=$("ko");out.textContent="";
 try{
  const {img,file}=await getImage("kf");
  const c=document.createElement("canvas");c.width=img.naturalWidth;c.height=img.naturalHeight;
  c.getContext("2d").drawImage(img,0,0);
  const b=await blobFromCanvas(c,"image/jpeg",Number($("quality").value)/100);
  download(b,baseName(file)+"-compressed.jpg");
  const pct=file.size?Math.round((1-b.size/file.size)*100):0;
  out.textContent=`Original: ${(file.size/1024).toFixed(1)} KB · New: ${(b.size/1024).toFixed(1)} KB · ${pct>=0?pct+"% smaller":Math.abs(pct)+"% larger"}.`;
 }catch(e){out.textContent=e.message}
};

$("wt").oninput=()=>{
 const t=$("wt").value;
 $("wo").textContent=`Words: ${t.trim()?t.trim().split(/\s+/u).length:0} · Characters: ${t.length} · Without spaces: ${t.replace(/\s/g,"").length} · Lines: ${t?t.split(/\r\n|\r|\n/).length:0}`;
};

document.querySelectorAll("[data-case]").forEach(btn=>btn.onclick=()=>{
 const el=$("ct"),v=el.value,m=btn.dataset.case;
 if(m==="upper")el.value=v.toUpperCase();
 if(m==="lower")el.value=v.toLowerCase();
 if(m==="title")el.value=v.toLowerCase().replace(/\b\p{L}/gu,x=>x.toUpperCase());
 if(m==="sentence")el.value=v.toLowerCase().replace(/(^\s*\p{L}|[.!?]\s+\p{L})/gu,x=>x.toUpperCase());
 $("cmsg").textContent="Text converted.";
});
async function copyText(value,out){
 try{await navigator.clipboard.writeText(value);out.textContent="Copied to clipboard."}
 catch(e){out.textContent="Copy was blocked. Select the result and press Ctrl+C."}
}
$("ccopy").onclick=()=>copyText($("ct").value,$("cmsg"));

$("clean").onclick=()=>{
 $("sr").value=$("st").value.trim().replace(/[ \t]+/g," ").replace(/\n{3,}/g,"\n\n");
 $("smsg").textContent="Text cleaned.";
};
$("scopy").onclick=()=>copyText($("sr").value,$("smsg"));

function processJSON(minify){
 const out=$("jo");
 try{
  $("jt").value=JSON.stringify(JSON.parse($("jt").value),nullify=>nullify);
 }catch(e){}
 try{
  const value=JSON.parse($("jt").value);
  $("jt").value=JSON.stringify(value,minify?null: null,minify?undefined:2);
  out.textContent="Valid JSON. "+(minify?"Minified.":"Formatted.");
 }catch(e){out.textContent="Invalid JSON: "+e.message}
}
$("jf").onclick=()=>{
 try{$("jt").value=JSON.stringify(JSON.parse($("jt").value),null,2);$("jo").textContent="Valid JSON — formatted successfully."}
 catch(e){$("jo").textContent="Invalid JSON: "+e.message}
};
$("jm").onclick=()=>{
 try{$("jt").value=JSON.stringify(JSON.parse($("jt").value));$("jo").textContent="Valid JSON — minified successfully."}
 catch(e){$("jo").textContent="Invalid JSON: "+e.message}
};

$("be").onclick=()=>{
 try{$("bt").value=btoa(unescape(encodeURIComponent($("bt").value)));$("bo").textContent="Encoded to Base64."}
 catch(e){$("bo").textContent="Could not encode text."}
};
$("bd").onclick=()=>{
 try{$("bt").value=decodeURIComponent(escape(atob($("bt").value.trim())));$("bo").textContent="Decoded successfully."}
 catch(e){$("bo").textContent="Invalid Base64 input."}
};
</script>
</body>
</html>

