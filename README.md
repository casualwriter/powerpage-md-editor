# Powerpage Markdown Editor

``Powerpage Markdown Editor`` is a markdown editor using [**Powerpage**](https://github.com/casualwriter/powerpage) with 
 js library of [*simplemde-markdown-editor*](https://github.com/sparksuite/simplemde-markdown-editor). 
 
 It is a simple html/js application demonstrating developing application using [Powerpage](https://github.com/casualwriter/powerpage).

![Powerpage Markdown Editor](powerpage-md.jpg)


## Installation & Run

* No installation is needed, Just download and run ``powerpage.exe``.
* The package is same as [Powerpage](https://github.com/casualwriter/powerpage), only ``powerpage.ini`` is revised.
* Commandline Syntax: ``powerpage.exe {filename}``, e.g. ``powerpage.exe  README.md``

## Source Code

It is single html/js program ([pp-markdown.html](source/pp-markdown.html)) within 100 lines, showing in below code-block. 

```
<!DOCTYPE html>
<html>
<title>Powerpage Markdown Editor</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/simplemde/latest/simplemde.min.css">
<script src="https://cdn.jsdelivr.net/simplemde/latest/simplemde.min.js"></script>
<style>
.CodeMirror {	height:calc(100vh - 160px); }
@media print{
  #header, .CodeMirror-scroll, .CodeMirror-vscrollbar, .editor-toolbar, .editor-statusbar { display:none!important }
  .CodeMirror, .editor-preview { position:relative; height:auto; overflow:hidden }
}
</style>
<body style="font-family:calibri">
  <div id=header style="padding:8px">
    <span style="font-size:20px;font-weight:700;">Markdown Editor </span>
    <a target=_new href="https://github.com/sparksuite/simplemde-markdown-editor">(SimpleMDE)</a>&nbsp;&nbsp;&nbsp;&nbsp;
    <b>File: </b><span id=filename style="color:green">new file</span>  
    <span style="float:right">
    <button onclick="window.location='powerpage.html'">PowerPage</button>&nbsp;&nbsp;&nbsp;
    <button onclick="pb.callback('onOpenFile').file.opendialog('Markdown (*.md),*md')" accesskey=o><b>O</b>pen</button>
    <button onclick="onPrint()" accesskey=p><b>P</b>rint</button>&nbsp;&nbsp;&nbsp;
    <button onclick="onSave()" accesskey=s><b>S</b>ave</button>
    <button onclick="pb.callback('onSaveAs').file.savedialog('Markdown (*.md),*md')" assesskey=a>Save <b>A</b>s</button>
    </span> 
  </div>
  <textarea id="content"></textarea>
</body>

<script>
// initial simple MDE (https://github.com/sparksuite/simplemde-markdown-editor)
var simplemde = new SimpleMDE( { element: document.getElementById("content") } );
var mdFile = document.getElementById("filename").innerText
var mdText = ''

// open file
function onOpenFile(result, type, url) {
  var rs = JSON.parse( result.split('\\').join('\\\\') )
  if (rs.status>0) {
    pb.callback('onLoadFile').file.read(rs.path)
    document.getElementById("filename").innerText = mdFile = (rs.path||mdFile)
    pb.microhelp( 'File ' + mdFile + ' loaded.' )
  }   
}

// load file
function onLoadFile(result, type, url) {
  simplemde.value( mdText = result )
  if (simplemde.isPreviewActive()) document.getElementsByClassName('fa fa-eye')[0].click()
}

// save file
function onSave() {
  if (mdFile == 'new file') {
    pb.callback('onSaveAs').file.savedialog('Markdown (*.md),*md')
  } else if ( simplemde.value() == mdText ) {
    alert('Data no change. no need to save!')
  } else {
    mdText = simplemde.value()
    pb.file.write( mdFile, '@mdText', 'onAfterSave' )
  }  
}

function onAfterSave(result, type, url) {
  alert('File has been saved to ' + mdFile + '!')
}

// saveas
function onSaveAs(result, type, url) {
  var rs = JSON.parse( result.split('\\').join('\\\\') )
  if (rs.status>0) {
    mdFile = (rs.path.substr(-3)=='.md') ? rs.path : rs.path+'.md'
    document.getElementById("filename").innerText = mdFile 
    onSave()
  }  
}

// When page ready. open file if has commandline parm (i.e. "powerpage.exe {markdown-file}")
function onPageReady(cmdline) {
   if (cmdline>' ') onOpenFile( '{"status":2, "path":"' + cmdline + '" }' ) 
}

// When close window, prompt to save data
function onPageClose() {
  if ( simplemde.value() !== mdText && mdFile !== 'new file' && confirm('Data has been change. Save Data?')) {
    mdText = simplemde.value()
    pb.file.write( mdFile, '@mdText', 'close' )
    return 'no'
  }
  //return 'yes'  
}

// print preview (active preview first)
function onPrint() {
  if (!simplemde.isPreviewActive()) document.getElementsByClassName('fa fa-eye')[0].click()
  setTimeout( function() { pb.print('preview') }, 500); 
}
</script>
```


## Modification History

* 2021/05/14, v0.50, first version with powerpage v0.43
* 2021/05/30, v0.55, add print preview function
* 2021/06/16, v0.60, print preview, commandline, etc.. (using powerpage v0.50)


## License

MIT






