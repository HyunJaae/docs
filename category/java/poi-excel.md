---
description: POI 라이브러리를 이용하여 엑셀의 인쇄 영역을 구현하는 방법 입니다.
---

# POI Excel 인쇄 영역, 페이지 나누기 설정

```java
sheet.lockSelectLockedCells(true); // 셀 잠금
sheet.getCTWorksheet().getSheetViews().getSheetViewArray(0).setView(PAGE_BREAK_PREVIEW); // 시트 보기를 페이지 나누기 미리보기로 설정
sheet.setDisplayGridlines(false); // 시트 그리드 표시 유무
PrintSetup print = sheet.getPrintSetup();
print.setPaperSize(PrintSetup.A4_PAPERSIZE); // 인쇄 페이지 설정
print.setScale((short) 80); // 인쇄 비율 설정(80%)
print.setFitHeight((short) 1); // 인쇄 시 시트를 한 페이지 높이에 맞추도록 설정
print.setFitWidth((short) 1); // 인쇄 시 시트를 한 페이지 너비에 맞추도록 설정
workbook.setPrintArea(0, "$A$1:$F$42"); // 첫번째 시트에 대해 인쇄 영역 설정
```

A1 셀부터 F42 셀까지 인쇄 영역으로 설정하는 방법이다.

시트의 그리드 표시 유무도 설정할 수 있다.
