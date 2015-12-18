


// HelpDlg.cpp : implementation file
//

#include "stdafx.h"
#include "Tetris.h"
#include "HelpDlg.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#undef THIS_FILE
static char THIS_FILE[] = __FILE__;
#endif

/////////////////////////////////////////////////////////////////////////////
// CHelpDlg dialog


CHelpDlg::CHelpDlg(CWnd* pParent /*=NULL*/)
	: CDialog(CHelpDlg::IDD, pParent)
{
	//{{AFX_DATA_INIT(CHelpDlg)
		// NOTE: the ClassWizard will add member initialization here
	//}}AFX_DATA_INIT
}


void CHelpDlg::DoDataExchange(CDataExchange* pDX)
{
	CDialog::DoDataExchange(pDX);
	//{{AFX_DATA_MAP(CHelpDlg)
		// NOTE: the ClassWizard will add DDX and DDV calls here
	//}}AFX_DATA_MAP
}


BEGIN_MESSAGE_MAP(CHelpDlg, CDialog)
	//{{AFX_MSG_MAP(CHelpDlg)
	//}}AFX_MSG_MAP
END_MESSAGE_MAP()

/////////////////////////////////////////////////////////////////////////////
// CHelpDlg message handlers

void CHelpDlg::OnOK() 
{
	// TODO: Add extra validation here
	
	CDialog::OnOK();
}


// HeroDlg.cpp : implementation file
//

#include "stdafx.h"
#include "Tetris.h"
#include "HeroDlg.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#undef THIS_FILE
static char THIS_FILE[] = __FILE__;
#endif

/////////////////////////////////////////////////////////////////////////////
// CHeroDlg dialog


CHeroDlg::CHeroDlg(CWnd* pParent /*=NULL*/)
	: CDialog(CHeroDlg::IDD, pParent)
{
	m_bWriteflg = FALSE;
}


void CHeroDlg::DoDataExchange(CDataExchange* pDX)
{
	CDialog::DoDataExchange(pDX);
	//{{AFX_DATA_MAP(CHeroDlg)
	DDX_Text(pDX, IDC_LEVEL_EDIT, m_level);
	DDX_Text(pDX, IDC_NAME_EDIT, m_name);
	DDX_Text(pDX, IDC_SCORE_EDIT, m_score);
	DDV_MinMaxInt(pDX, m_score, 0, 10000);
	//}}AFX_DATA_MAP
}


BEGIN_MESSAGE_MAP(CHeroDlg, CDialog)
	//{{AFX_MSG_MAP(CHeroDlg)
	ON_BN_CLICKED(IDOK_BTN, OnBtn)
	//}}AFX_MSG_MAP
END_MESSAGE_MAP()

/////////////////////////////////////////////////////////////////////////////
// CHeroDlg message handlers

void CHeroDlg::OnOK() 
{

}

void CHeroDlg::SetWriteFlg(BOOL bflg)
{
	m_bWriteflg = bflg;
}

int CHeroDlg::DoModal() 
{
	char pszTmp[128] = {0};

	//读取配置文件
	GetPrivateProfileString("HERO", "name", "0", 
		pszTmp, 127, ".\\setup.ini");
	m_name = CString(pszTmp);

	if(!m_bWriteflg)
	{
		GetPrivateProfileString("HERO", "score", "0", 
			pszTmp, 127, ".\\setup.ini");
		m_score = atoi(pszTmp);
		GetPrivateProfileString("HERO", "level", "0", 
			pszTmp, 127, ".\\setup.ini");
		m_level = atoi(pszTmp);
		
	}

	return CDialog::DoModal();
}

void CHeroDlg::OnBtn() 
{
	UpdateData(TRUE);
	if(m_bWriteflg)
	{
		CString tmp;
		tmp.Format("%d", m_score);
		WritePrivateProfileString("HERO", "name", m_name, ".\\setup.ini");
		WritePrivateProfileString("HERO", "score", tmp, ".\\setup.ini");
		tmp.Format("%d", m_level);
		WritePrivateProfileString("HERO", "level", tmp, ".\\setup.ini");
	}
	m_bWriteflg = FALSE;

	CDialog::OnOK();
}

BOOL CHeroDlg::OnInitDialog() 
{
	CDialog::OnInitDialog();
	
	if(m_bWriteflg)
	{
		SetDlgItemText(IDOK_BTN, "记录");
	}
	
	return TRUE; 
}


// LevelDlg.cpp : implementation file
//

#include "stdafx.h"
#include "Tetris.h"
#include "LevelDlg.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#undef THIS_FILE
static char THIS_FILE[] = __FILE__;
#endif

/////////////////////////////////////////////////////////////////////////////
// CLevelDlg dialog


CLevelDlg::CLevelDlg(CWnd* pParent /*=NULL*/)
	: CDialog(CLevelDlg::IDD, pParent)
{
	//{{AFX_DATA_INIT(CLevelDlg)
	m_level = 0;
	//}}AFX_DATA_INIT
}


void CLevelDlg::DoDataExchange(CDataExchange* pDX)
{
	CDialog::DoDataExchange(pDX);
	//{{AFX_DATA_MAP(CLevelDlg)
	DDX_Text(pDX, IDC_LEVEL_EDIT, m_level);
	DDV_MinMaxInt(pDX, m_level, 1, 10);
	//}}AFX_DATA_MAP
}


BEGIN_MESSAGE_MAP(CLevelDlg, CDialog)
	//{{AFX_MSG_MAP(CLevelDlg)
	//}}AFX_MSG_MAP
END_MESSAGE_MAP()

/////////////////////////////////////////////////////////////////////////////
// CLevelDlg message handlers

void CLevelDlg::OnOK() 
{
	if(UpdateData(TRUE))
	{
		CString tmp;
		tmp.Format("%d", m_level);
		WritePrivateProfileString("SETUP", "level", tmp, ".\\setup.ini");
		CDialog::OnOK();
	}
}

void CLevelDlg::OnCancel() 
{

	CDialog::OnCancel();
}

BOOL CLevelDlg::OnInitDialog() 
{
	CDialog::OnInitDialog();
	
	char pszTmp[128] = {0};

	GetPrivateProfileString("SETUP", "level", "0", 
		pszTmp, 127, ".\\setup.ini");
	m_level = atoi(pszTmp);

	UpdateData(FALSE);

	return TRUE;  // return TRUE unless you set the focus to a control
	              // EXCEPTION: OCX Property Pages should return FALSE
}


// MainFrm.cpp : implementation of the CMainFrame class
//

#include "stdafx.h"
#include "Tetris.h"

#include "MainFrm.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#undef THIS_FILE
static char THIS_FILE[] = __FILE__;
#endif

/////////////////////////////////////////////////////////////////////////////
// CMainFrame

IMPLEMENT_DYNCREATE(CMainFrame, CFrameWnd)

BEGIN_MESSAGE_MAP(CMainFrame, CFrameWnd)
	//{{AFX_MSG_MAP(CMainFrame)
		// NOTE - the ClassWizard will add and remove mapping macros here.
		//    DO NOT EDIT what you see in these blocks of generated code !
	ON_WM_CREATE()
	//}}AFX_MSG_MAP
END_MESSAGE_MAP()

static UINT indicators[] =
{
	ID_SEPARATOR,           // status line indicator
	ID_INDICATOR_CAPS,
	ID_INDICATOR_NUM,
	ID_INDICATOR_SCRL,
};

/////////////////////////////////////////////////////////////////////////////
// CMainFrame construction/destruction

CMainFrame::CMainFrame()
{
	// TODO: add member initialization code here
	
}

CMainFrame::~CMainFrame()
{
}

int CMainFrame::OnCreate(LPCREATESTRUCT lpCreateStruct)
{
	if (CFrameWnd::OnCreate(lpCreateStruct) == -1)
		return -1;
	return 0;
}

BOOL CMainFrame::PreCreateWindow(CREATESTRUCT& cs)
{
	if( !CFrameWnd::PreCreateWindow(cs) )
		return FALSE;

	cs.cx=550;
	cs.cy=590;
	return TRUE;
}

/////////////////////////////////////////////////////////////////////////////
// CMainFrame diagnostics

#ifdef _DEBUG
void CMainFrame::AssertValid() const
{
	CFrameWnd::AssertValid();
}

void CMainFrame::Dump(CDumpContext& dc) const
{
	CFrameWnd::Dump(dc);
}

#endif //_DEBUG

/////////////////////////////////////////////////////////////////////////////
// CMainFrame message handlers


/////////////////////////////////////////////////////////////////////////////
//Rule.cpp

#include "stdafx.h"
#include "Rule.h"

CRule::CRule()
{
}

CRule::~CRule()
{
}

void CRule::SetLevel(int nLevel)
{
	m_nLevel = nLevel;
}

int CRule::UpLevel(int nLine)
{
	if(nLine / 30)
	{
		m_nLevel++;
	}
	return m_nLevel;
}

bool CRule::Win(int Now[4][4], int Russia [100][100], CPoint NowPosition)
{
	if(m_nLevel == 11)
	{//消除行数已经超过10级,游戏结束
		return true;
	}

	for(int i=0;i<4;i++)
	{
		for(int j=0;j<4;j++)
		{
			if(Now[i][j]==1)
			{//到了顶点
				if(Russia[i+NowPosition.x][j+NowPosition.y]==1)
				{
					return true;
				}
			}
		}
	}

	return false;
}


#include "stdafx.h"
#include "Tetris.h"
#include "Russia.h"

#include "HeroDlg.h"

//////////////////////////////////////////////////////////////////////////
//构造函数
//////////////////////////////////////////////////////////////////////////
CRussia::CRussia()
{
	bkMap.LoadBitmap(IDB_BACK);
	fkMap.LoadBitmap(IDB_FANGKUAI);
}

//////////////////////////////////////////////////////////////////////////
//析构函数
//////////////////////////////////////////////////////////////////////////
CRussia::~CRussia()
{
}
//////////////////////////////////////////////////////////////////////////
//行消除函数
//////////////////////////////////////////////////////////////////////////
void CRussia::LineDelete()
{
	int m=0;		//本次共消去的行数
	bool flag=0;
	for(int i=0;i<m_RowCount;i++)
	{
		//检查要不要消行
		flag=true;
		for(int j=0;j<m_ColCount;j++)
		{
			if(Russia[i][j]==0)
			{
				flag=false;
			}
		}

		//如果要
		if(flag==true)
		{
			m++;
			for(int k=i;k>0;k--)
			{
				//上行给下行
				for(int l=0;l<m_ColCount;l++)
				{
					Russia[k][l]=Russia[k-1][l];
				}
			}
			//第一行为零
			for(int l=0;l<m_ColCount;l++)
			{
				Russia[0][l]=0;
			}
		}
	}

	DrawWill();
	//加分
	switch(m)
	{
	case 1:
		m_Score= m_Score + 10 + m_Level * 10;
		break;
	case 2:
		m_Score= m_Score + 30 + m_Level * 10;
		break;
	case 3:
		m_Score= m_Score + 50 + m_Level * 10;
		break;
	case 4:
		m_Score= m_Score + 100 + m_Level * 10;
		break;
	default:
		break;
	}
	
	m_CountLine+=m;

	m_Level = rule.UpLevel(m_CountLine);

	end = rule.Win(Now, Russia, NowPosition);

	//速度
	m_Speed=320 - m_Level * 20;
	
	if(end)
	{
		HeroWrite();
	}

}
//////////////////////////////////////////////////////////////////////////
//移动方块
//////////////////////////////////////////////////////////////////////////
void CRussia::Move(int direction)
{
	if(end) return;
	
	switch(direction)
	{
		//左
	case KEY_LEFT:
		if(Meet(Now,KEY_LEFT,NowPosition)) break;
		NowPosition.y--;
		break;
		//右
	case KEY_RIGHT:
		if(Meet(Now,KEY_RIGHT,NowPosition)) break;
		NowPosition.y++;
		break;
		//下
	case KEY_DOWN:
		if(Meet(Now,KEY_DOWN,NowPosition))
		{
			LineDelete();			
			break;
		}
		NowPosition.x++;
		break;
		//上
	case KEY_UP:
		Meet(Now,KEY_UP,NowPosition);
		break;
	default:
		break;
	}
}
//////////////////////////////////////////////////////////////////////////
//方块旋转
//////////////////////////////////////////////////////////////////////////
bool CRussia::Change(int a[][4],CPoint p,int  b[][100])
{
	int tmp[4][4];
	int i,j;
	int k=4,l=4;
	
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			tmp[i][j]=a[j][3-i];
			After[i][j]=0;	//存放变换后的方块矩阵
		}
	}
	
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			if(tmp[i][j]==1)
			{
				if(k>i) k=i;
				if(l>j) l=j;
			}
		}
	}
	for(i=k;i<4;i++)
	{
		for(j=l;j<4;j++)
		{
			After[i-k][j-l]=tmp[i][j];
		}	//把变换后的矩阵移到左上角
	}
	//判断是否接触，是：返回失败
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{	
			if(After[i][j]==0)
			{
				continue;
			}
			if(((p.x+i)>=m_RowCount)||((p.y+j)<0)||((p.y+j)>=m_ColCount))
			{
				return false;
			}
			if(b[p.x+i][p.y+j]==1)
			{
				return false;
			}
		}
	}
	return true;
}
//////////////////////////////////////////////////////////////////////////
//判碰撞,遇到了边界或者有其他方块档住
//////////////////////////////////////////////////////////////////////////
bool CRussia::Meet(int a[][4],int direction,CPoint p)
{
	int i,j;
	//先把原位置清0 
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			if(a[i][j]==1)
			{
				Russia[p.x+i][p.y+j]=0;
			}
		}
	}
	
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			if(a[i][j]==1)
			{
				switch(direction)
				{
				case 1:	//左移
					if((p.y+j-1)<0) goto exit;
					if(Russia[p.x+i][p.y+j-1]==1) goto exit;
					break;
				case 2://右移
					if((p.y+j+1)>=m_ColCount) goto exit;
					if(Russia[p.x+i][p.y+j+1]==1) goto exit;
					break;
				case 3://下移
					if((p.x+i+1)>=m_RowCount) goto exit;
					if(Russia[p.x+i+1][p.y+j]==1) goto exit;
					break;
				case 4://变换
					if(!Change(a,p,Russia)) goto exit;				
					for(i=0;i<4;i++)
					{
						for(j=0;j<4;j++)
						{
							Now[i][j]=After[i][j];
							a[i][j]=Now[i][j];
						}
					}
					return false;
					break;
				}
			}
		}
	}
			
	int x,y;
	x=p.x;
	y=p.y;
	//移动位置，重新给数组赋值
	switch(direction)
	{
	case 1:
		y--;break;
	case 2:
		y++;break;
	case 3:
		x++;break;
	case 4:
		break;
	}
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			if(a[i][j]==1)
			{
				Russia[x+i][y+j]=1;
			}
		}
	}
			
	return false;
exit:
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			if(a[i][j]==1)
			{
				Russia[p.x+i][p.y+j]=1;
			}
		}
	}
	return true;	
}
//////////////////////////////////////////////////////////////////////////
//绘将出现的方块图
//////////////////////////////////////////////////////////////////////////
void CRussia::DrawWill()
{
	int i,j;
	int k=4,l=4;
	
    //把将要出现的方块给当前数组，并把将要出现数组赋值为零
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			Now[i][j]=Will[i][j];
			Will[i][j]=0;
		}
	}
	//初始化随即数种子
	srand(GetTickCount());
	int nTemp=rand()%Count;
	//各种图形
	switch(nTemp)
	{
	case 0:
		Will[0][0]=1;
		Will[0][1]=1;
		Will[1][0]=1;
		Will[1][1]=1;
		break;
	case 1:
		Will[0][0]=1;
		Will[0][1]=1;
		Will[1][0]=1;
		Will[2][0]=1;
		break;
	case 2:
		Will[0][0]=1;
		Will[0][1]=1;
		Will[1][1]=1;
		Will[2][1]=1;
		break;
	case 3:
		Will[0][1]=1;
		Will[1][0]=1;
		Will[1][1]=1;
		Will[2][0]=1;
		break;
	case 4:
		Will[0][0]=1;
		Will[1][0]=1;
		Will[1][1]=1;
		Will[2][1]=1;
		break;
	case 5:
		Will[0][0]=1;
		Will[1][0]=1;
		Will[1][1]=1;
		Will[2][0]=1;
		break;
	case 6:
		Will[0][0]=1;
		Will[1][0]=1;
		Will[2][0]=1;
		Will[3][0]=1;
		break;
	default:
		break;
	}
	
	int tmp[4][4];
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			tmp[i][j]=Will[j][3-i];
		}
	}
	
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			if(tmp[i][j]==1)
			{
				if(k>i) k=i;
				if(l>j) l=j;
			}
		}
	}
	
	for(i=0;i<4;i++)
	{
		for(j=0;j<4;j++)
		{
			Will[i][j]=0;
		}
	}
	//把变换后的矩阵移到左上角
	for(i=k;i<4;i++)
	{
		for(j=l;j<4;j++)
		{
			Will[i-k][j-l]=tmp[i][j];
		}
	}
	//开始位置
	NowPosition.x=0;
	NowPosition.y=m_ColCount/2;
}
//////////////////////////////////////////////////////////////////////////
//绘游戏界面
//////////////////////////////////////////////////////////////////////////
void CRussia::DrawBK(CDC*pDC)
{
	CDC Dc;
	if(Dc.CreateCompatibleDC(pDC)==FALSE)
	{
		AfxMessageBox("Can't create DC");
	}
	//画背景
    Dc.SelectObject(bkMap);
	pDC->BitBlt(0,0,540,550,&Dc,0,0,SRCCOPY);
    //画分数，速度，难度
	DrawScore(pDC);
    //如果有方块，显示方块
	//游戏区
	for(int i=0;i<m_RowCount;i++)
	{
		for(int j=0;j<m_ColCount;j++)
		{
			if(Russia[i][j]==1)
			{
				Dc.SelectObject(fkMap);
				pDC->BitBlt(j*30,i*30,30,30,&Dc,0,0,SRCCOPY);
			}
		}
	}
	//预先图形
	for(int n=0;n<4;n++)
	{
		for(int m=0;m<4;m++)
		{
			if(Will[n][m]==1)
			{	
				Dc.SelectObject(fkMap);
				pDC->BitBlt(365+m*30,240+n*30,30,30,&Dc,0,0,SRCCOPY);
			}
		}
	}
}
//////////////////////////////////////////////////////////////////////////
//绘分数和等级
//////////////////////////////////////////////////////////////////////////
void CRussia::DrawScore(CDC*pDC)
{
	int nOldDC=pDC->SaveDC();	
	//设置字体
	CFont font;    
	if(0==font.CreatePointFont(300,"Comic Sans MS"))
	{
		AfxMessageBox("Can't Create Font");
	}
	pDC->SelectObject(&font);
    //设置字体颜色及其背景颜色
	CString str;
	pDC->SetTextColor(RGB(39,244,10));
	pDC->SetBkColor(RGB(255,255,0));
    //输出数字
	str.Format("%d",m_Level);
	if(m_Level>=0)
		pDC->TextOut(420,120,str);

	str.Format("%d",m_CountLine);	
	if(m_Speed>=0)	
		pDC->TextOut(420,64,str);
	
	str.Format("%d",m_Score);	
	if(m_Score>=0)
		pDC->TextOut(420,2,str);
	
	pDC->RestoreDC(nOldDC);
}
//////////////////////////////////////////////////////////////////////////
//游戏开始
//////////////////////////////////////////////////////////////////////////
void CRussia::GameStart()
{
	end=false;//运行结束标志
    m_Score=0;		//初始分数
	m_RowCount=18;	//行数
	m_ColCount=12;	//列数
	Count=7;		//方块种类
	m_CountLine = 0;//合计消除行数为0

	char pszTmp[128] = {0};
					//读取当前游戏等级
	GetPrivateProfileString("SETUP", "level", "1", 
			pszTmp, 127, ".\\setup.ini");

	m_Level = atoi(pszTmp);		//初始等级
	m_Speed=320 - m_Level * 20;	//初始速度
	rule.SetLevel(m_Level);

	for(int i=0;i<m_RowCount;i++)
	{
		for(int j=0;j<m_ColCount;j++)
		{
			Russia[i][j]=0;
		}
	}	
	for(i=0;i<4;i++)
	{
		for(int j=0;j<4;j++)
		{
			Now[i][j]=0;
			Will[i][j]=0;
		}
	}
	//开始时将要出现方块没有生成,其不能赋值给当前方块数组，所以连续调用两次
	DrawWill();	
	DrawWill();
}

void CRussia::HeroWrite()
{
	CHeroDlg dlg;

	char pszTmp[128] = {0};

	int nHighScore = 0;
	
	GetPrivateProfileString("HERO", "score", "0", 
		pszTmp, 127, ".\\hero.ini");

	nHighScore = atoi(pszTmp);

	if(m_Score>nHighScore)
	{
		
		dlg.SetWriteFlg(TRUE);		//设置可写入标志
		
		dlg.m_level = m_Level;		//设置等级
		
		dlg.m_score = m_Score;		//设置分数
		
		dlg.DoModal();				//弹出对话框
	}
	else
	{
		AfxMessageBox("游戏结束,您未能进入英雄榜!");
	}

}


// Tetris.cpp : Defines the class behaviors for the application.
//

#include "stdafx.h"
#include "Tetris.h"

#include "MainFrm.h"
#include "TetrisDoc.h"
#include "TetrisView.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#undef THIS_FILE
static char THIS_FILE[] = __FILE__;
#endif

/////////////////////////////////////////////////////////////////////////////
// CTetrisApp

BEGIN_MESSAGE_MAP(CTetrisApp, CWinApp)
	//{{AFX_MSG_MAP(CTetrisApp)
	ON_COMMAND(ID_APP_ABOUT, OnAppAbout)
		// NOTE - the ClassWizard will add and remove mapping macros here.
		//    DO NOT EDIT what you see in these blocks of generated code!
	//}}AFX_MSG_MAP
	// Standard file based document commands
	ON_COMMAND(ID_FILE_NEW, CWinApp::OnFileNew)
	ON_COMMAND(ID_FILE_OPEN, CWinApp::OnFileOpen)
	// Standard print setup command
	ON_COMMAND(ID_FILE_PRINT_SETUP, CWinApp::OnFilePrintSetup)
END_MESSAGE_MAP()

/////////////////////////////////////////////////////////////////////////////
// CTetrisApp construction

CTetrisApp::CTetrisApp()
{
	// TODO: add construction code here,
	// Place all significant initialization in InitInstance
}

/////////////////////////////////////////////////////////////////////////////
// The one and only CTetrisApp object

CTetrisApp theApp;

/////////////////////////////////////////////////////////////////////////////
// CTetrisApp initialization

BOOL CTetrisApp::InitInstance()
{
	AfxEnableControlContainer();

	// Standard initialization
	// If you are not using these features and wish to reduce the size
	//  of your final executable, you should remove from the following
	//  the specific initialization routines you do not need.

#ifdef _AFXDLL
	Enable3dControls();			// Call this when using MFC in a shared DLL
#else
	Enable3dControlsStatic();	// Call this when linking to MFC statically
#endif

	// Change the registry key under which our settings are stored.
	// TODO: You should modify this string to be something appropriate
	// such as the name of your company or organization.
	SetRegistryKey(_T("Local AppWizard-Generated Applications"));

	LoadStdProfileSettings();  // Load standard INI file options (including MRU)

	// Register the application's document templates.  Document templates
	//  serve as the connection between documents, frame windows and views.

	CSingleDocTemplate* pDocTemplate;
	pDocTemplate = new CSingleDocTemplate(
		IDR_MAINFRAME,
		RUNTIME_CLASS(CTetrisDoc),
		RUNTIME_CLASS(CMainFrame),       // main SDI frame window
		RUNTIME_CLASS(CTetrisView));
	AddDocTemplate(pDocTemplate);

	// Parse command line for standard shell commands, DDE, file open
	CCommandLineInfo cmdInfo;
	ParseCommandLine(cmdInfo);

	// Dispatch commands specified on the command line
	if (!ProcessShellCommand(cmdInfo))
		return FALSE;

	// The one and only window has been initialized, so show and update it.
	m_pMainWnd->ShowWindow(SW_SHOW);
	m_pMainWnd->UpdateWindow();

	return TRUE;
}




CAboutDlg::CAboutDlg() : CDialog(CAboutDlg::IDD)
{
	//{{AFX_DATA_INIT(CAboutDlg)
	//}}AFX_DATA_INIT
}

void CAboutDlg::DoDataExchange(CDataExchange* pDX)
{
	CDialog::DoDataExchange(pDX);
	//{{AFX_DATA_MAP(CAboutDlg)
	//}}AFX_DATA_MAP
}

BEGIN_MESSAGE_MAP(CAboutDlg, CDialog)
	//{{AFX_MSG_MAP(CAboutDlg)
		// No message handlers
	//}}AFX_MSG_MAP
END_MESSAGE_MAP()

// App command to run the dialog
void CTetrisApp::OnAppAbout()
{
	CAboutDlg aboutDlg;
	aboutDlg.DoModal();
}

/////////////////////////////////////////////////////////////////////////////
// CTetrisApp message handlers



// TetrisView.cpp : implementation of the CTetrisView class
//

#include "stdafx.h"
#include "Tetris.h"

#include "TetrisDoc.h"
#include "TetrisView.h"

#include "HelpDlg.h"
#include "HeroDlg.h"
#include "LevelDlg.h"
#include "Russia.h"

#include <mmsystem.h>

#ifdef _DEBUG
#define new DEBUG_NEW
#undef THIS_FILE
static char THIS_FILE[] = __FILE__;
#endif

/////////////////////////////////////////////////////////////////////////////
// CTetrisView

IMPLEMENT_DYNCREATE(CTetrisView, CView)

BEGIN_MESSAGE_MAP(CTetrisView, CView)
	//{{AFX_MSG_MAP(CTetrisView)
	ON_COMMAND(IDR_ABOUT, OnAbout)
	ON_COMMAND(IDR_HERO_LIST, OnHeroList)
	ON_COMMAND(IDR_LEVEL_SETUP, OnLevelSetup)
	ON_COMMAND(IDR_PLAY_MUSIC, OnPlayMusic)
	ON_COMMAND(IDR_START_GAME, OnStartGame)
	ON_COMMAND(IDR_HELP, OnHelp)
	ON_WM_KEYDOWN()
	ON_WM_TIMER()
	//}}AFX_MSG_MAP
	// Standard printing commands
	ON_COMMAND(ID_FILE_PRINT, CView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_DIRECT, CView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_PREVIEW, CView::OnFilePrintPreview)
END_MESSAGE_MAP()

/////////////////////////////////////////////////////////////////////////////
// CTetrisView construction/destruction

CTetrisView::CTetrisView()
{
	m_bStart = FALSE;
}

CTetrisView::~CTetrisView()
{
}

BOOL CTetrisView::PreCreateWindow(CREATESTRUCT& cs)
{
	// TODO: Modify the Window class or styles here by modifying
	//  the CREATESTRUCT cs

	return CView::PreCreateWindow(cs);
}

/////////////////////////////////////////////////////////////////////////////
// CTetrisView drawing

void CTetrisView::OnDraw(CDC* pDC)
{
	CTetrisDoc* pDoc = GetDocument();
	ASSERT_VALID(pDoc);
	CDC Dc;
	if(Dc.CreateCompatibleDC(pDC)==FALSE)
		AfxMessageBox("Can't create DC");
	//没有开始，显示封面
	if( m_bStart)
	{
		russia.DrawBK(pDC);
	}

}

/////////////////////////////////////////////////////////////////////////////
// CTetrisView printing

BOOL CTetrisView::OnPreparePrinting(CPrintInfo* pInfo)
{
	// default preparation
	return DoPreparePrinting(pInfo);
}

void CTetrisView::OnBeginPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: add extra initialization before printing
}

void CTetrisView::OnEndPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: add cleanup after printing
}

/////////////////////////////////////////////////////////////////////////////
// CTetrisView diagnostics

#ifdef _DEBUG
void CTetrisView::AssertValid() const
{
	CView::AssertValid();
}

void CTetrisView::Dump(CDumpContext& dc) const
{
	CView::Dump(dc);
}

CTetrisDoc* CTetrisView::GetDocument() // non-debug version is inline
{
	ASSERT(m_pDocument->IsKindOf(RUNTIME_CLASS(CTetrisDoc)));
	return (CTetrisDoc*)m_pDocument;
}
#endif //_DEBUG

/////////////////////////////////////////////////////////////////////////////
// CTetrisView message handlers
void CTetrisView::OnAbout() 
{
	CAboutDlg aboutDlg;		//生成关于对话框
	aboutDlg.DoModal();		//弹出关于对话框
}

void CTetrisView::OnHeroList() 
{
	CHeroDlg dlg;			//生成英雄榜对话框
	dlg.DoModal();			//弹出英雄榜对话框
}

void CTetrisView::OnLevelSetup() 
{
	CLevelDlg	dlg;	//生成等级设置对话框
	dlg.DoModal();			//弹出等级设置对话框
}

void CTetrisView::OnPlayMusic() 
{
	CWnd*   pMain   =   AfxGetMainWnd();   
	CMenu*   pMenu   =   pMain->GetMenu();
	//判断播放音乐菜单当前状态
	BOOL bCheck = (BOOL)pMenu->GetMenuState(IDR_PLAY_MUSIC, MF_CHECKED);
	
	if(m_bStart)
	{
		if(bCheck)
		{
			pMenu->CheckMenuItem(IDR_PLAY_MUSIC, MF_BYCOMMAND | MF_UNCHECKED);
		}
		else
		{
			pMenu->CheckMenuItem(IDR_PLAY_MUSIC, MF_BYCOMMAND | MF_CHECKED);
		}
		
		PlayBackMusic(!bCheck);			//调用播放背景音乐功能函数
	}

}

void CTetrisView::OnStartGame() 
{
	m_bStart = true;
	russia.GameStart();					//调用RUSSIA对象的游戏开始函数
	SetTimer(1, russia.m_Speed, NULL);
}

void CTetrisView::OnHelp() 
{
	CHelpDlg dlg;						//生成帮助对话框
	dlg.DoModal();						//弹出对话框
}

void CTetrisView::PlayBackMusic(BOOL bCheck)
{
	//指定文件并播放
	if(bCheck)
	{								//播放音乐
		sndPlaySound("music.wav",SND_ASYNC); 
	}
	else
	{								//停止播放
		sndPlaySound(NULL,SND_PURGE); 
	}

}

void CTetrisView::OnKeyDown(UINT nChar, UINT nRepCnt, UINT nFlags) 
{
	//没有开始
	if(!m_bStart)
		return;
	
	switch(nChar)
	{
	case VK_LEFT:
		russia.Move(KEY_LEFT);
		break;
	case VK_RIGHT:
		russia.Move(KEY_RIGHT);
		break;		
	case VK_UP:
		russia.Move(KEY_UP);
		break;
	case VK_DOWN:
		russia.Move(KEY_DOWN);
		break;
	}
	//重画
	CDC* pDC=GetDC();
	russia.DrawBK(pDC);
	ReleaseDC(pDC);

	CView::OnKeyDown(nChar, nRepCnt, nFlags);
}

void CTetrisView::OnTimer(UINT nIDEvent) 
{
	//下移
	russia.Move(KEY_DOWN);
	//重画
	russia.DrawBK(GetDC());
	//关闭TIME1
	KillTimer(1);
	//调整速度
	SetTimer(1, russia.m_Speed, NULL);

	CView::OnTimer(nIDEvent);
}
