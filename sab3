function getFileIdsInFolder() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var folderId = ss.getSheetByName("Mail Data").getRange("E2").getValue(); //change with your Sheet name & fill the folder ID in the desired Range
  var folder = DriveApp.getFolderById(folderId);
  var files = folder.getFiles();
  var sheet = ss.getSheetByName("Base Data"); //change with your Sheet name
  var fileIds = [];
  var fileNames = [];
  
  while (files.hasNext()) {
    var file = files.next();
    if (file.getMimeType() === MimeType.MICROSOFT_EXCEL) {
      fileIds.push(file.getId());
      fileNames.push(file.getName());
      for (var i = 0; i < fileIds.length; i++) {
      sheet.getRange(i + 2, 2).setValue(fileIds[i]);
      sheet.getRange(i + 2, 1).setValue(fileNames[i]);
      }
    }
  }

  Logger.log("File IDs: " + fileIds.join(", "));
  return fileIds;
}


function sendmail() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getSheetByName("Base Data");
  var startRow = 2;
  var dataRange = sheet.getRange(startRow, 1, sheet.getLastRow()-1, 4);
  var data = dataRange.getValues();
  
  for (var j = 0; j < data.length; j++) {
    var row = data[j]; 
    var emailAddress = row[2];
    var emailAddress2 = row[3]; //if an address must be put in CC of the email
    var fileId = row[1];
    var file = DriveApp.getFileById(fileId);
    var cond = row[0];	
    
    if (cond != "") 
    {
      var alias = GmailApp.getAliases(); //if the mail will be sent via an Alias set in Gmail account
      var subject = "STOCKS"; //change with your subject
      var message = "Hello, \r\n\r\n........ \r\n....... \r\n....... \r\n\r\n....., \r\n....."; //type your message

     GmailApp.sendEmail(emailAddress, subject, message, {from: alias[0], cc: emailAddress2, attachments: [file.getAs(MimeType.MICROSOFT_EXCEL)]}); 
    }
    }
  }
