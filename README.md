AIX
=
● VG, LV, FS 생성
-

```
lspv	
```
VG를 생성 할 PV 확인
```
bootinfo -s hdisk2		
```
PV 용량 확인
```
smitty vg		
```
볼륨그룹 생성
- Add a Volume Group
- Add an Original Volume Group
	VOLUME GROUP name                                  [oraclevg]
	PHYSICAL VOLUME names                              [hdisk1]  
	Force the creation of a volume group?               yes
```
smitty lv		[[lv생성]]
```
- Add a Logical Volume
- VOLUME GROUP name                                  [oraclevg]
- Number of LOGICAL PARTITIONS                       [50]		PP용량 확인 후 개수 
- PHYSICAL VOLUME names                              [hdisk1] 
- Logical volume TYPE                                [jfs2] 
```
lsvg -o | lsvg -il		[[vg, lv 생성 확인]]
```
```
smitty jfs2		[[파일시스템 생성]]
```
- Add an Enhanced Journaled File System

  SIZE of file system
          Unit Size                                   Gigabytes                                                                                    
          Number of units                            [100]                                                                                          
  MOUNT POINT                                        [/oracle]
  Mount AUTOMATICALLY at system restart?              yes                   
```
mount /oracle [[파일시스템 마운트]]
```
```
df -gt	[[파일시스템 확인]]
```

● VG, LV, FS 제거
-
```
umount /oracle
```

```
smitty jfs2
```
- Remove an Enhanced Journaled File System
- FILE SYSTEM name                                                                                                                                 
- Remove Mount Point                                  yes      

```
smitty lv
```
- Remove a Logical Volume
- LOGICAL VOLUME name                                [lv선택]

```
smitty vg
```
- Remove a Volume Group
- VOLUME GROUP name                                  [oraclevg]   


