function myFunction() {
  var token = 'xxx'
  var ss = SpreadsheetApp.openById('xxx')
  var sh = ss.getSheetByName('xxx')
  var row = sh.getLastRow();

  var today = Utilities.formatDate(new Date(), "GMT+7", "dd/MM/yyyy")
  var time = Utilities.formatDate(new Date(), "GMT+7", "HH:mm")

  for (i = 2; i <= row; i++) {
    var date = Utilities.formatDate(sh.getRange(i, 1).getValue(), "GMT+7", "dd/MM/yyyy")
    var timer = sh.getRange(i, 4).getValue()

    if (today == date && time == timer) {
      var msg1 = sh.getRange(i, 2).getValue() + '\n'
      var msg2 = sh.getRange(i, 3).getValue() + '\n'
      var message = '\n ชื่อ สกุล : ' + msg1 + ' เวร : ' + msg2
      sendLineNotify(message, token)
    }
  }
}

function sendLineNotify(message, token) {
  var options = {
    "method": "post",
    "payload": {
      "message": message,
    },
    "headers": { "Authorization": "Bearer " + token }
  };
  UrlFetchApp.fetch("https://notify-api.line.me/api/notify", options);
}
