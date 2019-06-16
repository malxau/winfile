DEBUG=0
PDB=0

RSC=rc.exe
LINK32=link.exe

OUTDIR=.\Release
INTDIR=.\Release
CPP_OBJS=$(INTDIR)/

ALL : "$(OUTDIR)\WinFile.exe"

CLEAN : 
	-@rd /s/q $(INTDIR) 2>NUL
	-@rd /s/q $(OUTDIR) 2>NUL

"$(OUTDIR)" :
    @if not exist "$(OUTDIR)/" mkdir "$(OUTDIR)"

CFLAGS=-nologo -Gz -MD -W3

CDEFS=/D "WIN32" /D "NDEBUG" /D "_WINDOWS" /D "FASTMOVE" /D "LFNCLIPBOARD" \
	  /D "MEMDOUBLE" /D "STRICT" /D "UNICODE" /D "_UNICODE" /D "USELASTDOT" \
	  /D _WIN32_WINNT=0x0400 /D WINVER=0x0400 
LIBS=kernel32.lib user32.lib gdi32.lib comctl32.lib comdlg32.lib advapi32.lib \
	 shell32.lib uuid.lib
LDFLAGS=-nologo -subsystem:windows -incremental:no -out:"$(OUTDIR)/WinFile.exe"

!IF $(DEBUG)==1
CFLAGS=$(CFLAGS) -Gy -Zi -Od
!ELSE
CFLAGS=$(CFLAGS) -Gy -O1sb1
!IF $(PDB)==1
CFLAGS=$(CFLAGS) -Zi
!ENDIF
!ENDIF

!IF $(PDB)==1
LDFLAGS=$(LDFLAGS) -DEBUG -pdb:"$(OUTDIR)/WinFile.pdb"
!ENDIF

#
# Probe for more esoteric aspects of compiler behavior.
# Apparently pragma doesn't cut it for -GS-
#
!IF [$(CC) -GS- 2>&1 | find "unknown" >NUL]>0
CFLAGS=$(CFLAGS) -GS- -GF -EHsc
!ELSE
!IF [$(CC) -GF 2>&1 | find "unknown" >NUL]>0
CFLAGS=$(CFLAGS) -GF -GX
!ELSE
CFLAGS=$(CFLAGS) -Gf -GX
!ENDIF
!ENDIF

CFLAGS=$(CFLAGS) $(CDEFS) /Fo"$(INTDIR)/" /c 

RSC_PROJ=/l 0x809 /fo"$(INTDIR)/res.res" /d "NDEBUG" 

OBJS= \
	"$(INTDIR)\dbg.obj" \
	"$(INTDIR)\lfn.obj" \
	"$(INTDIR)\lfnmisc.obj" \
	"$(INTDIR)\numfmt.obj" \
	"$(INTDIR)\res.res" \
	"$(INTDIR)\suggest.obj" \
	"$(INTDIR)\tbar.obj" \
	"$(INTDIR)\treectl.obj" \
	"$(INTDIR)\wfassoc.obj" \
	"$(INTDIR)\wfchgnot.obj" \
	"$(INTDIR)\wfcomman.obj" \
	"$(INTDIR)\wfcopy.obj" \
	"$(INTDIR)\wfdir.obj" \
	"$(INTDIR)\wfdirrd.obj" \
	"$(INTDIR)\wfdirsrc.obj" \
	"$(INTDIR)\wfdlgs.obj" \
	"$(INTDIR)\wfdlgs2.obj" \
	"$(INTDIR)\wfdlgs3.obj" \
	"$(INTDIR)\wfdos.obj" \
	"$(INTDIR)\wfdrives.obj" \
	"$(INTDIR)\wfext.obj" \
	"$(INTDIR)\wffile.obj" \
	"$(INTDIR)\wfinfo.obj" \
	"$(INTDIR)\wfinit.obj" \
	"$(INTDIR)\wfmem.obj" \
	"$(INTDIR)\wfprint.obj" \
	"$(INTDIR)\wfsearch.obj" \
	"$(INTDIR)\wftree.obj" \
	"$(INTDIR)\wfutil.obj" \
	"$(INTDIR)\winfile.obj" \
	"$(INTDIR)\wnetcaps.obj"

"$(OUTDIR)\WinFile.exe" : "$(OUTDIR)" $(DEF_FILE) $(OBJS)
    @$(LINK32) @<<
  $(LDFLAGS) $(OBJS) $(LIBS)
<<

.c{$(CPP_OBJS)}.obj:
   @$(CC) $(CFLAGS) $(CDEFS) $<  

.rc{$(CPP_OBJS)}.res:
   @$(RSC) $(RSC_PROJ) $<

