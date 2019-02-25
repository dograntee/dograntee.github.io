---
layout: post
title: "5. OllyDBG 플러그인 SDK"
date: 2019-02-24 12:33:00
image: '/assets/img/reversing/intro.PNG'
description: Reverse Engineering Bible
category: 'reversing'
tags:
- reversing
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: A summary of my study
---
## 00x플러그인 도입부

~~~c

extc int _export cdecl ODBG_Plugininit(
  int ollydbgversion,HWND hw,ulong *features) 
{
	// OllyDBG와 Plugin SDK 버전을 비교하여
	// Plugin이 더 높다면 -1 리턴하여 실행시키지 않음
	if (ollydbgversion<PLUGIN_VERSION)
		return -1;

	// OllyDBG의 메인 윈도우 핸들을 받아놓는다. 
	// 이 핸들은 메시지박스등을 출력하기 위해 반드시 필요하다	
	hwmain=hw;

	// Log Window에 문자열을 출력한다.
	Addtolist(0, 0, "Hello Olly  plugin v0.01 (test plugin)");
	Addtolist(0, -1, "made by window31");	

	return 0;
}

~~~

위 함수들을 사용하여 메인 메뉴에 등록 가능.


## 01x 하위 메뉴 작성

~~~c

extc int _export cdecl ODBG_Pluginmenu(int origin,char data[4096],void *item) 
{
	switch (origin) 
	{
	case PM_MAIN: // Plugin menu in main window
		strcpy(data,
			"0 &Hello Olly Dialog,"
			"1 Test|"
			"2 About"
			);
		return 1;
	
	case PM_DISASM:
		if (Getstatus() == STAT_NONE) 
		{
			return 0;
		}
		strcpy(data,"0 &Hello Olly Dialog");
		return 1;
	
	case PM_THREADS:
		if (Getstatus() == STAT_NONE) 
		{
			return 0;
		}
		strcpy(data,"0 &Hello Olly Dialog");
		return 1;
	
	default:
		break; // Any other window
	};
	return 0; // Window not supported by plugin
};
~~~

## 02x 메뉴 핸들러 작성

~~~c

extc void _export cdecl ODBG_Pluginaction(int origin,int action,void *item) {
	int id;	
	
	switch(origin) 
	{
	case PM_MAIN:
	case PM_DISASM:
		switch (action) 
		{
		case MENU_HELLOOLLY_DIALOG:
			// 프로세스가 Attach 되지 않았으면 에러 출력
			if (Getstatus() == STAT_NONE) 
			{
				MessageBox(hwmain, "No process!!", "ERR", MB_OK);
				return;
			}
			
			id = DialogBox(hinst, MAKEINTRESOURCE(IDD_HELLOOLLY), hwmain, (DLGPROC)MainDlgProc);
            // 인스턴스 핸들, 템플릿 어떤거?, 실행하는 윈도우, 프로시저(다이얼로그 안에서 동작)
			if (id == IDOK) 
			{
				//Doit(hwmain);
			}			
			
			break;
		case MENU_TEST:
			MessageBox(hwmain, "TEST Selection", "TEST", MB_OK);
			
			break;
		case MENU_ABOUT:			
			MessageBox(hwmain, "Hello Olly ver 0.01", "TEST", MB_OK);

			break;
		default:
			break;
		}
	}
}
~~~

## 03x 다이얼로그 내부

~~~c

LRESULT CALLBACK MainDlgProc(HWND hDlgWnd, UINT msg, WPARAM wp, LPARAM lp)
{
   HDC hDC;
   PAINTSTRUCT ps;      	

	RECT  rect;
	UINT  x,y,w,h,xMax,yMax;

	switch (msg) 
	{
		case WM_INITDIALOG:
			// 다이얼로그를 한가운데에 출력
			GetWindowRect(hDlgWnd, &rect);
			h = rect.bottom - rect.top;
			w = rect.right  - rect.left;

			xMax = GetSystemMetrics(SM_CXMAXIMIZED);
			yMax = GetSystemMetrics(SM_CYMAXIMIZED);
			
			x = xMax/2 - w/2;
			y = yMax/2 - h;
			
			MoveWindow(hDlgWnd, x, y, w, h, TRUE);
			break;

		case WM_COMMAND:
			switch (LOWORD(wp)) 
			{
			case IDOK:

				EndDialog(hDlgWnd, IDOK);
				break;

			case IDCANCEL:
				EndDialog(hDlgWnd, IDCANCEL);
				break;

			default:
				break;
			}
		case WM_PAINT:			
			hDC = BeginPaint(hDlgWnd, &ps);
			EndPaint(hDlgWnd, &ps);
			break;
			
		default:
			break;
	}
	
	return 0;
}

~~~

## 04x 종료 처리

~~~c

extc int _export cdecl ODBG_Pluginclose(void) 
{
	//if(lpMyAlloc) 
	//{
	//	FreeMyAlloc();
	//	lpMyAlloc = NULL;
		Addtolist(0, -1, "== Close Hello Plugin ==");
	//}
	return 0;
}

~~~

## 05x t_dump 구조체

 ![problem](/assets/img/reversing/5/fig1.PNG "example for t_dump")
<center><font size="0.5em">(Fig 1. exmple for t_dump)</font></center><br>

디스어셈블 상태에서 일정 번지를 블록으로 설정한 뒤 팝업 메뉴를 통해 플러그인을 실행시키면 내용이 채워진다.

~~~c

extc void _export cdecl ODBG_Pluginaction(int origin,int action,void *item) {
	int id;	
	
	switch(origin) 
	{
	case PM_MAIN:
	case PM_DISASM:
		switch (action) 
		{
		case MENU_HELLOOLLY_DIALOG:
			// 프로세스가 Attach 되지 않았으면 에러 출력
			if (Getstatus() == STAT_NONE) 
			{
				MessageBox(hwmain, "No process!!", "ERR", MB_OK);
				return;
			}

			CheckStruct(item);
			//구조체 내용을 stderr에 출력. 내용 아래 사진
			/*
			id = DialogBox(hinst, MAKEINTRESOURCE(IDD_HELLOOLLY), hwmain, (DLGPROC)MainDlgProc);
			if (id == IDOK) 
			{
				//Doit(hwmain);
			}
			*/
			
			break;
		case MENU_IDP:
			//BypassIDP();
			MessageBox(0,0,0,0);
			CheckStruct(item);
			
			

			
			break;
		case MENU_ABOUT:			
			MessageBox(hwmain, "Hello Olly ver 0.01", "TEST", MB_OK);

			break;
		default:
			break;
		}
	}
}

~~~

 ![problem](/assets/img/reversing/5/fig2.PNG "example for t_dump description")
<center><font size="0.5em">(Fig 2. exmple for t_dump description)</font></center><br>

 - item : 4CD6A8
  item 구조체가 할당된 메모리 영역

 - threadid : 29C
  threadid는 현재 프로세스의 스레드 ID(피디버깅 중 프로세스)

 - sel0 : 402D09.sel1:402D31
  sel0는 영역의 첫번째, sel1는 영역의 마지막

 - base : 401000
  현재 프로그램의 base 주소

 - size : 3000
  .text 영역의 크기

## 06x t_memory 구조체

 ![problem](/assets/img/reversing/5/fig3.PNG "example for t_memory")
<center><font size="0.5em">(Fig 2. exmple for t_memory)</font></center><br>

Findmemory 함수의 인자로 메모리 번지를 넘겨주면 해당 메모리 번지의 정보를 나타낸다.

 - base : 401000

 - size : 3000

 - type : 100000
  메모리 번지의 타입 값 헤더파일에 선언되어 있음(100000은 .text).
 - sect : .text
  메모리 번지 값이 100000일 때 sect가 .text가 아니면 패킹된 것을 유추가능.

## 07x t_disasm 구조체

 - Readmemory(IpAddress, size, MM_RESTORE|MM_SILENT)
 어태치 된 프로세스의 메모리를 읽어온다.
 
 - Disasm(uchar *src, ulong srcip, uchar *srcdec,
            t_disasm *disasm, int disasmmode,ulong threadid)
  4번째 인자에 구조체를 넣어주면 해당 변수에 값을 담아온다.

Readmemory로 메모리 영역을 읽어오고 다시 Disasm에 src로 넘겨주면 disasm 구조체가 채워진다.

 - dump, result:
  opcode의 헥사값과 디스어셈블된 코드

 - comment:
  사용자 주석 정보

 - cmdtype : 70
   현재 옵코드의 종류(0x70은 CALL)

 - jmpaddr : 402e25
   다른 메모리 번지 이동이 있을 경우 출력.

~~~c

void copyInstructions(void *item)
{
  t_dump		*pd;
  t_memory		*pmem;
  t_bookmark	mark, *pb, *pb2;
  t_disasm		da;


  unsigned int	line=0,pocetBytup=0,NumberOfInstructions=0,i,j;
  ulong			cmdsize;
  char			cmd[MAXCMDSIZE],*pdecode;
  ulong			decodesize;

	line=0;
	pd=(t_dump *)item;
	pocetBytu=0;
	NumberOfInstructions=0;

	cmdsize=MAXCMDSIZE;
	// clear the bookamrk
	//Deletesorteddatarange(&(bookmark.data),0,0xFFFFFFFF);

	startC=pd->sel0;
	endC=pd->sel1;

	while(pd->sel1>(pd->sel0+pocetBytu)){
		NumberOfInstructions++;
		Readmemory(cmd,pd->sel0+pocetBytu,cmdsize,MM_RESTORE|MM_SILENT);
		pmem=Findmemory(pd->sel0+pocetBytu);
		pdecode=Finddecode(pd->sel0+pocetBytu,&decodesize);
		pocetBytu+=Disasm(cmd,cmdsize,(pd->sel0)+pocetBytu,pdecode,&da,DISASM_SIZE,0);
	}
	
	if (Createsorteddata(&(bookmark.data),"ExtraCopy",
	sizeof(t_bookmark),NumberOfInstructions,NULL,NULL)!=0)	
	{	
		MessageBox(hwmain,"Unable to allocate memory for the block","Info",MB_OK);
		return ;                         // Unable to allocate bookmark data
	}
	pocetBytu=0;
	for (i=0;i<NumberOfInstructions;i++){
		Readmemory(cmd,pd->sel0+pocetBytu,cmdsize,MM_RESTORE|MM_SILENT);
		pmem=Findmemory(pd->sel0+pocetBytu);
		pdecode=Finddecode(pd->sel0+pocetBytu,&decodesize);
		
		pocetBytup=pocetBytu;
		pocetBytu+=Disasm(cmd,cmdsize,(pd->sel0)+pocetBytu,pdecode,&da,DISASM_CODE,0);


		mark.index=line;
		mark.size=1;
		mark.type=0;
		mark.addr=pd->sel0+pocetBytup;
		mark.length=pocetBytu-pocetBytup;
		strcpy(mark.code,da.dump);
		strcpy(mark.ASM,da.result);		
		mark.JumpTo=da.jmpaddr;
		//set Jump To
		Addsorteddata(&(bookmark.data),&mark);
		if (bookmark.hw!=NULL) InvalidateRect(bookmark.hw,NULL,FALSE);
			line++;
	} //end while	
	
	NumberL=NumberOfInstructions;
	
	for(i=0;i<NumberL;i++){
		pb=(t_bookmark *)Findsorteddata(&(bookmark.data),i);
		pb->shift=0;
		pb->newSize=0;
		Findname(pb->addr,NM_COMMENT,pb->Comment);	

		if (pb!=NULL){
			if ((pb->JumpTo>=startC) && (pb->JumpTo<endC)){
				for(j=0;j<NumberL;j++){
					pb2=(t_bookmark *)Findsorteddata(&(bookmark.data),j);
					if (pb->JumpTo==pb2->addr){
						pb->JmpToIndex=j;
						break;
					}
				}
			}
			else pb->JmpToIndex=-1;
		}
	}
}

~~~