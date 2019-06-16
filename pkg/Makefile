
ARCH=win32

!IF [$(CC) -? 2>&1 | findstr /C:"for x64" >NUL 2>&1]==0
ARCH=amd64
!ELSE
!IF [$(CC) -? 2>&1 | findstr /C:"for AMD64" >NUL 2>&1]==0
ARCH=amd64
!ELSE
!IF [$(CC) -? 2>&1 | findstr /C:"for ARM64" >NUL 2>&1]==0
ARCH=arm64
!ENDIF
!ENDIF
!ENDIF

distribution: 
	@cd ..\src\Release
	@yori -c ypm -c ..\..\pkg\winfile-$(ARCH).cab winfile `date` $(ARCH) -filelist ..\..\pkg\winfile.lst -minimumosbuild 1381 -upgradepath http://www.malsmith.net/download/?obj=contrib/latest/winfile-$(ARCH).cab -symbolpath http://www.malsmith.net/download/?obj=contrib/latest/winfile-pdb-$(ARCH).cab -sourcepath http://www.malsmith.net/download/?obj=contrib/latest/winfile-source.cab
	@yori -c ypm -c ..\..\pkg\winfile-pdb-$(ARCH).cab winfile-pdb `date` $(ARCH) -filelist ..\..\pkg\winfile-pdb.lst -upgradepath http://www.malsmith.net/download/?obj=contrib/latest/winfile-pdb-$(ARCH).cab -sourcepath http://www.malsmith.net/download/?obj=contrib/latest/winfile-source.cab
	@cd ..\..
	@yori -c ypm -cs pkg\winfile-source.cab winfile-source `date` -filepath .
	@cd pkg

clean:
	@if exist *.cab erase *.cab
