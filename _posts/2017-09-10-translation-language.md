---
layout: post
title: 다국어 언어 지원 개발하기
update:  2017-09-10T21:00:00Z
published: true
complete: false
tags: android, language, 번역, 
excerpt: 다국어 언어 지원 개발은 다양한 고려대상이 있다.  
---
# 다국어 언어 지원 개발하기

## Intro
글로벌 소프트웨어 개발에 있어 다국어는 어떤 플랫폼, 어떤 환경에서도 늘 고려대상이다. 글로벌 서비스를 목표로 한다면 필수적으로 고려해야 하는데, 지원하려는 대상 국가가 많아진다던가, 같은 표현이라고 다르게 표현한다던가, 언어 특성 및 길이에 따라 축약 번역 등 **다국어 언어 작업은 다양한 사람들이 상당한 양의 업무를 수행해야 하게된다.** 

다국어 업무를 수행하는데 있어서 반드시 아래 내용은 고려되어야 한다고 생각한다.

* 여러 담당자들이 **한 공간에서 작업을 진행 할 수 있어야 한다.** 실시간으로 개발이 어떻게 진행되고 있는지, 반영은 언제 되었는지 등 히스토리가 잘 공유 될 수 있어야 한다. 
* 다량의 리소스를 **쉽게 소프트웨어에 적용** 할 수 있어야 한다.

--------------------
## 번역 업무 수행 환경 구성하기

다양한 작업자들이 한 곳에서 작업할 수 있는 환경을 위하여 **Google Drive - Sheet 문서**형식을 선택 했다. 
먼저, 공유하기 위한 문서를 작성하고 **History**, **Manual**, **Project** 시트를 추가하였다. 각 시트는 다음 내용을 담고 있다.


#### History 시트

![History 시트](/images/sheet_history.png){: width="100%"}

History 시트는 번역업무에 관련된 각 담당자들이 *어디까지 작업된 사항이 반영되어있는지, 어떠한 요구사항에 의하여 새로운 문구가 추가 / 수정 / 삭제 되었는지* 등을 알 수 있도록 정리한 시트이다. 번역 업무를 담당하는 주 담당자가 잘 관리해야 하며, 프로젝트에 **작업 진행중인 사항이 실제 반영이 되었는지를 알려주는 기록을 잘 확인**할 수 있도록 신경쓰는 것이 중요하다. 주로 개발 또는 요구사항을 전달하는 주체가 많이 기록하게 되는 시트이다.

#### Manual 시트

![Manual 시트](/images/sheet_manual.png){: width="100%"}

Manual 시트는 번역업무에 관련된 담당자들이 프로젝트에 번역에 대한 업무를 어떤식으로 진행할지 논의된 사항을 정리해 놓은 페이지이다. 특히 번역을 담당하는 담당자들이 보통 언어 1~2개에 1명이 담당하게 되므로, 지원하는 언어의 수가 많아질 수록 관련 담당자들도 많아지게 된다. 그렇기 때문에 번역업무를 어떻게 진행하는지를 알려주는 Manual 시트는 **번역 담당자들이 업무 프로세와 무엇을 해야 하는지에 대하여 이해하기 쉽게 잘 작성하는 것**이 중요하다.

#### 프로젝트 시트

![Project 시트](/images/sheet_project.png){: width="100%"}

Project 시트는 실제 리소스가 반영되어야 하는 프로젝트의 필요 리스트를 나열하는 시트이다.
1열에 리소스 키, 한국어, 번역되어야 할 대상을 차례로 추가해준다. **번역을 하는 언어기준이 한국어가 아니라면 다른 언어가 한국어 위치 대신 와도 상관없다.** 첨부 이미지는 Android 프로젝트에서 사용하는 리소스 기준으로 작성하였으며, **프로젝트 리소스가 사용자 채널마다 다르게 제공될 경우 (웹, iOS 등) 다르게 만들어서 관리**해도 상관없다.

--------------------------------------------------------------

## 리소스 생성 스크립트 작성 

#### 메뉴 추가하기

시트 문서에서 에서 실제 번역을 수행할 시트탭을 선택 하고 *'도구 > 스크립트 편집기'* 를 선택한다.
먼저 아래 코드를 입력하여 사용 편의를 위하여 구글 시트에 메뉴를 추가한다.

```javascript

function onOpen() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet();
  var entries = [
    { 
      name : "Android", 
      functionName : "createAndroidStringResource" 
    }
  ]; 
  sheet.addMenu("다국어 리소스 만들기", entries);
};

```

![메뉴 생성 이미지](/images/sheet_menu.png)

onOpen 함수에서 시트에 메뉴를 추가하는 간단한 코드이며, 각 메뉴가 동작할 function 을 지정한다. 프로젝트가 여러개이거나 기타 다양한 상황에 대하여 업무 편의를 위하여 메뉴를 다양하게 구성하면 된다.

#### 리소스 생성 script 추가

*'도구 > 스크립트 편집기'* 에서 메뉴 선택시 수행할 createAndroidStringResource 를 작성한다.

``` javascript
function createAndroidStringResource() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var resultMsg = '';
  if( sheet != null ) {
    Logger.log("Sheet name : " + sheet.getName() );
   
    // 메뉴에 해당되는 시트가 맞는지 체크
    if( sheet.getName() == '' ) {
      var sheetData = sheet.getSheetValues(1, 1, sheet.getLastRow(), sheet.getLastColumn());
      var targetObject = [
        [4, "values-zh-rTW"],       // 중국어(번체)
        [3, "values-zh-rCN"],       // 중국어(간체)
        [2, "values"],              // 영어
        [1, "values-ko"],           // 한국어
      ];
      var fileName = "strings.xml";
        
      for(var i=0; i<targetObject.length; i++) {
        var result = convertXmlString(sheetData, targetObject[i][0]);
        var folderPath = targetObject[i][1];
      
        if( result != null ) {
          DriveApp.createFolder(folderPath).createFile(fileName, result);
        }
      } 
      resultMsg = '리소스 생성 성공 !!';
    } else { 
      resultMsg = ''해당프로젝트' 시트를 선택하고 실행하세요.';
    }
  }
  SpreadsheetApp.getUi().alert(
    '알림',
    resultMsg,
    SpreadsheetApp.getUi().ButtonSet.OK
  );
}

function convertXmlString(sheetData, columnIndex) {
  Logger.log("Create Xml Resource, columnName : " + sheetData[0][columnIndex]);
  
  var result = "<resource>\r\n";
  for(var i=1; i<sheetData.length; i++) {
    var row = sheetData[i];
    var key = row[0];
    var value = row[columnIndex];
    
    if( value == null || value == '' ) {
      value = "";
    }
    
//    Logger.log("[" + i + "] key : " + key + ", value : " + value);
    if( key == null || key == '' ) {
      result += "\r\n";
    } else if( key.indexOf("<!--") > -1 ) {
      result += "\t" + key + "\r\n";
    } else {
      result += "\t" + "<string name=\"" + key + "\">\"" + value + "\"</string>\r\n";
    }
  }
  result += "</resource>"
              
//  Logger.log(result);
  return result;
}

```

**프로젝트 시트가 선택된 상태**에서 동작될 수 있도록 구현되어 있어, 메뉴 선택 후 Alert를 통해 몇 가지 예외추가를 처리하였다. 해당 스크립트까지 작성을 완료하고 위에서 추가한 리소스 생성 메뉴를 선택하면 현재 로그인된 계정의 구글드라이브에 strings.xml 이 폴더별로 생성된다.

![실행결과1](/images/sheet_result1.png){: width="100%"}
![실행결과2](/images/sheet_result2.png){: width="100%"}

생성된 리소스를 폴더별로 모두 선택하여 프로젝트의 각 언어별 폴더로 저장하면 반영 되며, **반영 후 History 시트에 반영 히스토리 내역을 작성**해준다.

~~그렇다면 iOS 용 파일 생성 스크립트는 ?~~


## 기타사항

* 언어 리소스가 자동으로 반영될 수 있는 내용도 있는데 이 부분은 추후에 작업 해보기로 ... 
* 문구가 조합되는 형태를 번역 담당자에게 잘 설명할 수 있어야 한다.  %s, %d는 어떤 값과 합쳐져서 표현된다고 알려줘야 하는데, 특히 Android 의 경우 %1\$s, %2\$s 와 같이 순서도 지정이 가능한 점 등을 잘 공유해야 한다.
* 하나의 프로젝트 시트탭으로 다양한 채널에 모두 적용할 수 있도록 하면 좋겠으나, OS나 마켓 정책 등으로 100% 동일한 문구를 사용하기 어려운 것으로 알고있다. ~~(번역 비용 OTL)~~ 
* 더 좋은 방법이 있다면 공유해주시면 감사하겠습니다 :)

-------------
참조 
* [Android언어 리소스 자동화](http://tiii.tistory.com/22)
* [Google Sheet Script Guide](https://developers.google.com/apps-script/guides/sheets)
 