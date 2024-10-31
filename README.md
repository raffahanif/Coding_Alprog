#Coding_Alprog

var SS = SpreadsheetApp.openById('MASUKKAN SHEET ID');
var hours = 0
var str = "";

function doPost(e) {

  var parsedData;
  var result = {};
  
  try { 
    parsedData = JSON.parse(e.postData.contents);
  } 
  catch(f){
    return ContentService.createTextOutput("Error in parsing request body: " + f.message);
  }
   
  if (parsedData !== undefined){
    var flag = parsedData.format;
    if (flag === undefined){
      flag = 0;
    }
    
    var sheet = SS.getSheetByName(parsedData.sheet_name); 
    var dataArr = parsedData.values.split(","); 
         
    //var date_now = Utilities.formatDate(new Date(), "CST", "yyyy/MM/dd"); 
    //var time_now = Utilities.formatDate(new Date(), "CST", "hh:mm:ss a"); 
    var Curr_Date = new Date(new Date().setHours(new Date().getHours() + hours));
    var Curr_Time = Utilities.formatDate(Curr_Date, "Asia/Jakarta", 'HH:mm:ss');

    var nama              = dataArr     [0]; 
    var nim               = dataArr     [1]; 
    var jurusan           = dataArr     [2]; 
    var prodi             = dataArr     [3];  
  
    switch (parsedData.command) {
      
      case "insert_row":
         
          var nextRow = sheet.getLastRow() + 1;
          sheet.getRange("A" + nextRow).setValue(Curr_Date);
          sheet.getRange("B" + nextRow).setValue(Curr_Time);
          sheet.getRange("C" + nextRow).setValue(nama);
          sheet.getRange("D" + nextRow).setValue(nim);
          sheet.getRange("E" + nextRow).setValue(jurusan);
          sheet.getRange("F" + nextRow).setValue(prodi);
         
         str = "Success"; 
         SpreadsheetApp.flush();
         break;
         
      case "append_row":
         
         var publish_array = new Array(); 
         
         publish_array [0] = nama;
         publish_array [1] = nim; 
         publish_array [2] = jurusan;  
         publish_array [3] = prodi;   
         
         sheet.appendRow(publish_array); 
         
         str = "Success"; 
         SpreadsheetApp.flush();
         break;     
 
    }
    
    return ContentService.createTextOutput(str);
  } // endif (parsedData !== undefined)
  
  else {
    return ContentService.createTextOutput("Error! Request body empty or in incorrect format.");
  }
}
