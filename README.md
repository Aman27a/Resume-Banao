<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Resume Banao</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.9.2/html2pdf.bundle.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">

<style>
body{
font-family:'Poppins',sans-serif;
margin:0;
background:#e4e7eb;
}

header{
text-align:center;
padding:15px;
font-size:28px;
font-weight:700;
letter-spacing:3px;
background:white;
}

.container{
display:flex;
flex-wrap:wrap;
gap:15px;
padding:15px;
}

.form-section{
flex:1;
min-width:320px;
background:white;
padding:15px;
border-radius:8px;
box-shadow:0 0 10px rgba(0,0,0,0.1);
max-height:90vh;
overflow:auto;
}

.preview-section{
flex:1;
min-width:320px;
display:flex;
justify-content:center;
}

input, textarea{
width:100%;
padding:6px;
margin-bottom:8px;
border-radius:4px;
border:1px solid #ccc;
font-size:13px;
}

button{
padding:6px 10px;
margin:4px 0;
background:#007bff;
color:white;
border:none;
border-radius:4px;
cursor:pointer;
font-size:12px;
}

button:hover{
background:#0056b3;
}

.remove-btn{
background:red;
margin-top:3px;
}

.resume{
width:794px;
min-height:1123px;
background:white;
padding:25px;
box-shadow:0 0 15px rgba(0,0,0,0.2);
font-size:13px;
line-height:1.4;
}

.resume-title{
text-align:center;
font-size:22px;
font-weight:700;
letter-spacing:4px;
margin-bottom:15px;
}

.name-line{
border-bottom:2px solid black;
margin:5px 0 10px 0;
}

.resume h3{
background:#dcdcdc;
padding:4px;
margin:10px 0 6px 0;
font-size:14px;
}

.photo{
width:110px;
height:130px;
object-fit:cover;
border:1px solid black;
float:right;
margin-left:10px;
display:none;
}

table{
width:100%;
border-collapse:collapse;
font-size:12px;
}

table, th, td{
border:1px solid black;
padding:4px;
text-align:center;
}

.exp-item{
margin-bottom:4px;
}
</style>
</head>

<body>

<header>RESUME</header>

<div class="container">

<div class="form-section">

<input type="file" accept="image/*" onchange="loadPhoto(event)">
<button type="button" onclick="removePhoto()">Remove Photo</button>

<input type="text" placeholder="Full Name" oninput="update('name',this.value)">
<textarea placeholder="Address" oninput="update('address',this.value)"></textarea>
<input type="text" placeholder="Mobile" oninput="update('mobile',this.value)">
<input type="email" placeholder="Email" oninput="update('email',this.value)">

<textarea placeholder="Career Objective" oninput="update('objective',this.value)"></textarea>

<h4>Academic Qualification</h4>
<div id="academicInputs"></div>
<button type="button" onclick="addAcademic()">+ Add Qualification</button>

<h4>Other Qualification</h4>
<div id="otherInputs"></div>
<button type="button" onclick="addOther()">+ Add Qualification</button>

<h4>Work Experience</h4>
<div id="experienceInputs"></div>
<button type="button" onclick="addExperience()">+ Add Experience</button>

<input type="text" placeholder="Father's Name" oninput="update('father',this.value)">
<input type="date" id="dobInput">
<input type="text" placeholder="Language Known" oninput="update('language',this.value)">
<input type="text" placeholder="Gender" oninput="update('gender',this.value)">
<input type="text" placeholder="Nationality" oninput="update('nationality',this.value)">
<input type="text" placeholder="Marital Status" oninput="update('marital',this.value)">
<input type="text" placeholder="Place" oninput="update('place',this.value)">

<textarea oninput="update('declaration',this.value)">
I hereby declare that the above information provided by me is true and correct to the best of my knowledge and belief.
</textarea>

<button onclick="downloadPDF()">Download PDF</button>
<button onclick="window.print()">Print Resume</button>

</div>

<div class="preview-section">
<div class="resume" id="resume">

<div class="resume-title">RESUME</div>

<img id="photoPreview" class="photo">

<h2 id="name"></h2>
<div class="name-line"></div>

<p id="address"></p>
<p><b>Mobile:</b> <span id="mobile"></span></p>
<p><b>Email:</b> <span id="email"></span></p>

<h3>CAREER OBJECTIVE</h3>
<p id="objective"></p>

<h3>ACADEMIC QUALIFICATION</h3>
<table>
<thead>
<tr>
<th>Sr. No</th>
<th>Qualification</th>
<th>Board</th>
<th>Year</th>
<th>Percentage</th>
</tr>
</thead>
<tbody id="academicTable"></tbody>
</table>

<h3>OTHER QUALIFICATION</h3>
<div id="otherPreview"></div>

<h3>WORK EXPERIENCE</h3>
<div id="experiencePreview"></div>

<h3>PERSONAL INFORMATION</h3>
<p><b>Father's Name:</b> <span id="father"></span></p>
<p><b>Date of Birth :</b> <span id="dob"></span></p>
<p><b>Language:</b> <span id="language"></span></p>
<p><b>Gender:</b> <span id="gender"></span></p>
<p><b>Nationality:</b> <span id="nationality"></span></p>
<p><b>Marital Status:</b> <span id="marital"></span></p>

<h3>DECLARATION</h3>
<p id="declaration"></p>

<div style="display:flex; justify-content:space-between; margin-top:20px;">
<div>
<p>Date: <span id="date"></span></p>
<p>Place: <span id="place"></span></p>
</div>
<div style="text-align:right;">
<p><b><span id="nameSignature"></span></b></p>
</div>
</div>

</div>
</div>

</div>

<script>

function update(id,value){
document.getElementById(id).innerText=value;
if(id==="name"){
document.getElementById("nameSignature").innerText=value;
}
}

document.getElementById("dobInput").addEventListener("change", function(){
let value=this.value;
if(value){
let date=new Date(value);
let day=String(date.getDate()).padStart(2,'0');
let month=String(date.getMonth()+1).padStart(2,'0');
let year=date.getFullYear();
document.getElementById("dob").innerText=day+"/"+month+"/"+year;
}
});

function loadPhoto(event){
let photo=document.getElementById("photoPreview");
if(event.target.files.length>0){
photo.src=URL.createObjectURL(event.target.files[0]);
photo.style.display="block";
}
}

function removePhoto(){
let photo=document.getElementById("photoPreview");
photo.src="";
photo.style.display="none";
document.querySelector("input[type='file']").value="";
}

function addAcademic(){
let container=document.getElementById("academicInputs");
let div=document.createElement("div");
div.innerHTML=`
<input placeholder="Qualification" oninput="updateAcademic()">
<input placeholder="Board" oninput="updateAcademic()">
<input placeholder="Year" oninput="updateAcademic()">
<input placeholder="Percentage" oninput="updateAcademic()">
<button type="button" class="remove-btn" onclick="this.parentElement.remove();updateAcademic()">Remove</button>`;
container.appendChild(div);
}

function updateAcademic(){
let rows=document.getElementById("academicInputs").children;
let table=document.getElementById("academicTable");
table.innerHTML="";
let count=1;
for(let r of rows){
let inputs=r.getElementsByTagName("input");
if(inputs[0].value!=""){
table.innerHTML+=`<tr>
<td>${count}</td>
<td>${inputs[0].value}</td>
<td>${inputs[1].value}</td>
<td>${inputs[2].value}</td>
<td>${inputs[3].value}</td>
</tr>`;
count++;
}
}
}

function addOther(){
let container=document.getElementById("otherInputs");
let div=document.createElement("div");
div.innerHTML=`
<input placeholder="Other Qualification Detail" oninput="updateOther()">
<button type="button" class="remove-btn" onclick="this.parentElement.remove();updateOther()">Remove</button>`;
container.appendChild(div);
}

function updateOther(){
let inputs=document.getElementById("otherInputs").getElementsByTagName("input");
let preview=document.getElementById("otherPreview");
preview.innerHTML="";
for(let i of inputs){
if(i.value!=""){
preview.innerHTML+=`<div class="exp-item">• ${i.value}</div>`;
}
}
}

function addExperience(){
let container=document.getElementById("experienceInputs");
let div=document.createElement("div");
div.innerHTML=`
<input placeholder="Experience Detail" oninput="updateExperience()">
<button type="button" class="remove-btn" onclick="this.parentElement.remove();updateExperience()">Remove</button>`;
container.appendChild(div);
}

function updateExperience(){
let inputs=document.getElementById("experienceInputs").getElementsByTagName("input");
let preview=document.getElementById("experiencePreview");
preview.innerHTML="";
for(let i of inputs){
if(i.value!=""){
preview.innerHTML+=`<div class="exp-item">• ${i.value}</div>`;
}
}
}

function downloadPDF(){
html2pdf().set({
margin:0,
filename:'Resume.pdf',
html2canvas:{scale:2},
jsPDF:{unit:'px',format:[794,1123],orientation:'portrait'}
}).from(document.getElementById("resume")).save();
}

document.getElementById("date").innerText=new Date().toLocaleDateString();
document.getElementById("declaration").innerText =
"I hereby declare that the above information provided by me is true and correct to the best of my knowledge and belief.";

</script>

</body>
</html>
