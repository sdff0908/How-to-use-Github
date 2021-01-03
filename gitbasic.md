

# git

> 분산버전관리시스템(DVCS)

최종 결과본만 저장되는 CVCS와 달리, 모든 이력(수정, 삭제 등)을 관리한다

## git commit author 설정

처음에 컴퓨터에서 git을 사용하려고 하면, 아래의 설정을 해야 commit이 가능하다

```bash
$ git config --global user.name __username__
$ git config --global user.email __email__
```

설정을 확인할 때는 아래의 명령어를 활용한다

```bash
$ git config --global -1
user.name=sdff0908
user.email=2khy0908@gmail.com
```

설정된 이메일과 Github에 등록된 이메일이 같아야 잔디밭이 칠해진다

## 로컬 저장소(repository) 설정

최초 작업 시

```bash
$ git init
```

`.git`폴더가 생성되고, 여기에 모든 git과 관련된 정보들이 저장된다

이후 작업 시 `(master)` 표시가 뜨는지 확인

```bash
Hwayeon Kim@DESKTOP-J281U8B MINGW64 ~/Desktop/새 폴더 (master)
```

## 기본작업 흐름

![gitbasic-1](https://user-images.githubusercontent.com/72610879/103472446-0dcd6400-4dd1-11eb-8b75-c579ee87101e.png)

### 1. add

> working directory의 변경사항을 staging area로 이동
>
> 커밋할 파일을 관리

```bash
$ git add . 							#. :현재 디렉토리(하위 디렉토리 포함) 전부
$ git add a.txt 							#확장자가 txt,파일이름 a
$ git add my_folder/ 						#특정 폴더
$ git add a.txt b.txt c.txt 					#여러 파일
```

### 2.commit

> 지금 상태를 스냅샷으로 찍음

```bash
$ git commit -m 'First commit'
```

커밋 메시지는 지금 기록하는 이력을 충분히 잘 나타낼 수 있도록 작성한다

## 상태 확인하기

### 1. status

> 로컬 저장소의  Working Directory(WD), Staging Area(SA) 상태 확인
>
> git저장소 내의 변경사항 추적

```bash
$ git status
```

* touch 이후

```bash
$ touch a.txt                					#touch: 파일 생성
$ git status		
On branch master
'''
No commits yet
Untracked files: 						#새로 생성된 파일(WD에 있는 파일)
'''
nothing added to commit but untracked files present (use "git add" to track)
```

* add 이후

```bash
$ git add .
$ git status
On branch master
'''
No commits yet
Changes to be committed: 					#커밋할 변경사항(SA에 있는 파일)
'''
```

* 파일 수정 또는 삭제 이후

```bash
$ git status
Changes not staged for commit:  				#SA에 있지 않은 변경사항(WD에 있는 파일)
'''
        modified:
'''
```

### 2. log

> 커밋 히스토리 보기
>
> 지금까지 기록된 커밋들을 확인할 수 있다

```bash
$ git log
commit 171b5dbcfd6694ff5d8566311809c63cb869f663 (HEAD -> master)
Author: sdff0908 <2khy0908@gmail.com>
Date:   Tue Dec 29 14:10:53 2020 +0900

    First commit

$ git log --oneline  						#각 커밋을 요약하여 출력
171b5db (HEAD -> master) First commit
$ git log -2							#가장 최근의 커밋 두개만 출력 
$ git log --oneline -1						#가장 최근의 커밋 한개만 요약하여 출력
```

## 원격저장소(remote repository) 활용

### 1. 준비

Github에 새 저장소(repository)를 만든다

이미 git에 저장된 파일을 수정한 경우라면 이 단계는 건너뛰고 3. push로 이동한다

### 2. 원격저장소 설정

깃, 원격저장소를(remote) 추가해줘(add). origin이라는 이름으로 URL!

```bash
$ git remote add origin __url__
```

아래의 명령어를 통해 설정된 원격저장소를 확인할 수 있다

```bash
$ git remote -v
```

### 3. push

origin 원격저장소의 master 브랜치로 push

```bash
$ git push origin master
```

### 4. clone

원격 저장소 데이터를 로컬 저장소로 복사한다

```bash
$ git clone __url__
```

원격 저장소 이름의 폴더가 생성되고, 해당 폴더로 이동하면 git을 활용할 수 있다

### 5. pull

원격 저장소 데이터를 로컬 저장소로 복사하여 로컬 저장소 데이터와 병합한다

```bash
$ git pull origin master
```

해당 파일의 최근 변경 사항을 알 수 있다

*clone은 처음 자료를 받을 때 사용하고, 이후로는 pull을 사용한다*

### 6. push 충돌

로컬 저장소의 커밋 이력과 원격 저장소의 커밋 이력이 다를 때 발생한다

![gitbasic-6](https://user-images.githubusercontent.com/72610879/103472366-0e192f80-4dd0-11eb-9d71-47d014cd6ee8.png)

```bash
$ git push origin master
'''
error: failed to push some refs to 
'''
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

원격 저장소의 변경사항을 로컬저장소로 가져와 병합한 후 (pull) 로컬 저장소의 변경사항을 원격저장소로 보내어(push) 오류를 해결한다(=*원격 저장소와 로컬 저장소의 커밋 이력을 같게 한다*)

![gitbasic-7](https://user-images.githubusercontent.com/72610879/103472367-0eb1c600-4dd0-11eb-97a8-043aa7492335.png)

```bash
$ git pull origin master
$ git push origin master
```

## 되돌리기

### 1. add 취소

```bash
$ git restore --staged a.txt
$ git status
On branch master
Untracked files:
'''
        a.txt
'''
```

### 2. WD 작업내용 취소

*주의! 커밋되지 않은 변경사항을 없애는 것으로 명령어를 실행한 이후 다시 돌이킬 수 없다*

```bash
$ git restore a.txt
$ git status
On branch master
nothing to commit, working tree clean
```

### 3. Commit 메시지 변경

*주의! 공개된 저장소에 이미 push가 된 경우 절대 변경 하지 않는다*

```bash
$ touch c.txt
$ git add .
$ git commit -m 'Add d.txt'
```

```bash
$ git log --oneline
5f5ac68 (HEAD -> master) Add d.txt
d81c176 Complete
fb4ad8d Add b.txt
ec0574d Add a.txt
$ git commit --amend
'''
vim 편집기로 수정 후 저장(esc :wq)
''' 
$ git log --oneline
0c330b4 (HEAD -> master) Add c.txt			   #커밋 해시값 변화
d81c176 Complete
fb4ad8d Add b.txt
ec0574d Add a.txt
```

### 4. Commit 취소

#### 1)reset

* `--hard` : 모든 작업(변경사항) 내용과 이력을 삭제 (조심!!!)
* `--soft` : 모든 변경사항을 SA에 보관
* `--mixed` : 모든 변경사항을 WD에 보관

```bash
$ git log --oneline
0c330b4 (HEAD -> master) Add d.txt
d81c176 Complete
fb4ad8d Add b.txt
ec0574d Add a.txt
$ git reset --hard d81c176    				   #d81c176 Complete 이후 단계 모두 지우기
HEAD is now at d81c176 Complete
$ git log --oneline
d81c176 (HEAD -> master) Complete
fb4ad8d Add b.txt
ec0574d Add a.txt
```

#### 2) revert 

```bash
$ git log --oneline
0c330b4 (HEAD -> master) Add d.txt
d81c176 Complete
fb4ad8d Add b.txt
ec0574d Add a.txt
$ git revert 0c330b4            			   #d.txt파일 삭제
'''
vim 편집기에서 변경사항 확인 후 저장(esc :wq)
''' 
Removing d.txt
[master 56ff1b7] Revert "Add d.txt"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 f.txt
$ git log --oneline
56ff1b7 (HEAD -> master) Revert "Add d.txt"   	           #commit을 취소한 이력 남음
0c330b4 Add d.txt
d81c176 Complete
fb4ad8d Add b.txt
ec0574d Add a.txt
```

## git으로 관리하고 싶지 않은 파일은 `.gitignore`에!

git 저장소 내 git으로 관리하고 싶지 않은 파일(예: 개인정보)이 있다면 `.gitignore` 안에 넣는다

`.gitignore` 안에 있는 파일은 커밋되지 않는다

```bash
$ touch .gitignore								
```

* `.gitignore`에 입력하기

```bash
# 특정파일
data.csv
# 특정폴더
images/
# 특정확장자
*.png
!profile.png 						   #특정 파일(profile.png)은 빼고
```

참고: https://gitignore.io

## branch

### 1. 명령어

#### 1) branch 생성

branch 생성은 commit 이후에 가능하다

```bash
$ git branch __branch name__
```

#### 2) branch 이동

```bash
$ git checkout __branch name__
Switched to branch  __branch name__
```

#### 3) branch 생성 및 이동

```bash
$ git checkout -b __branch name__
Switched to branch  __branch name__
```

#### 4) branch 목록

```bash
$ git branch
* __branch name2__ 					   # *:HEAD가 가리키는 위치
  __branch name1__
  master
```

#### 5) branch 병합

```bash
(master) $ git merge __branch name__
```

브랜치 병합은 항상 master 브랜치에서 이루어지므로 HEAD가 master 브랜치를 가리키는지 확인한다. 

#### 6) branch 삭제

```bash
$ git branch -d __branch name__
```

### 2. branch scenario

#### 1) fast-forward

> __ branch name __ 브랜치 생성 이후 master 브랜치에 변경 사항이 없는 상황

1.  practice브랜치 생성 및 이동

   ```bash
   $ git branch practice
   $ git branch
     practice
   * master
   $ git checkout practice
   Switched to branch 'practice'
   ```
   
2. 작업 완료 후 commit

   ```bash
   #practice브랜치에서 작업 후
   $ git add .
   $ git commit -m 'Complete data'
   '''
   $ git log --oneline
   5ff4709 (HEAD -> practice) Complete data	           #HEAD가 practice브랜치 가리킴
   c6f5db0 (master) Add README
   ```


3. master브랜치로 이동

   ```bash
   $ git checkout master
   Switched to branch 'master'
   ```


4. master브랜치에 병합

   ```bash
   $ git log --oneline
   c6f5db0 (HEAD -> master) Add README			   #HEAD가 master브랜치 가리킴
   $ git merge practice					   #master브랜치에 practice브랜치 병합
   Updating c6f5db0..5ff4709
   Fast-forward						   #master브랜치에 변경사항 없으므로 그냥 앞으로 쭉-
   '''
   ```


5. 결과 확인 

   단순히 HEAD를 이동했음을 알 수 있다

   ```bash
   $ git log --oneline 
   5ff4709 (HEAD -> master, practice) Complete data
   c6f5db0 Add README
   ```

6. branch 삭제

   ```bash
   $ git branch -d practice
   Deleted branch practice (was 5ff4709).
   ```

#### 2) merge commit

> 서로 다른 커밋 이력을 병합(merge)하는 과정에서 다른 파일이 수정되어 있는 상황

![gitbasic-2](https://user-images.githubusercontent.com/72610879/103472370-0f4a5c80-4dd0-11eb-9413-0e0d51bde2d1.png)

1. practice 브랜치 생성 및 이동

   ```bash
   $ git checkout -b practice
   Switched to a new branch 'practice'
   ```

2. 작업 완료 후 commit

   ```bash
   #practice브랜치에서 작업 후
   $ git add .
   $ git commit -m 'Complete data'
   '''
   $ git log --oneline
   6b0245e (HEAD -> practice) Complete data
   5ff4709 (master) Complete test
   c6f5db0 Add README
   ```

3. master브랜치로 이동

   ```bash
   $ git checkout master
   ```

4. *master브랜치에 추가 commit발생시키기*

   ```bash
   #master브랜치에서 작업 후
   $ git add .
   $ git commit -m 'Hotfix'
   $ git log --oneline
   6930e34 (HEAD -> master) Hotfix
   5ff4709 Complete test
   c6f5db0 Add README
   ```

5. master브랜치에 병합

   ```bash
   $ git merge practice
   ```
   
6. 자동으로 *merge commit 발생*

   vim 편집기 화면이 나타난다

   <img width="453" alt="gitbasic-3" src="https://user-images.githubusercontent.com/72610879/103472066-11aab780-4dcc-11eb-936a-de6fba593805.png">

   커밋 메시지 확인 후 `esc`와 `:wq`를 차례로 눌러 저장 및 종료한다

   * `w` : write
   * `q` : quit

7. 그래프로 결과 확인하기

   ```bash
   $ git log --oneline --graph
   *   44515f8 (HEAD -> master) Merge branch 'practice'
   |\
   | * 6b0245e (practice) Complete data
   * | 6930e34 Hotfix
   |/
   * 5ff4709 Complete test
   * c6f5db0 Add README
   ```

8. branch 삭제

   ```bash
   $ git branch -d practice
   Deleted branch practice (was 6b0245e).
   ```


#### 3) merge commit 충돌

> 서로 다른 커밋 이력을 병합(merge)하는 과정에서 동일 파일이 수정되어 있는 상황

![gitbasic-4](https://user-images.githubusercontent.com/72610879/103472373-0fe2f300-4dd0-11eb-979c-86da653112c2.PNG)

1. practice 브랜치 생성 및 이동

   ```bash
   $ git checkout -b practice
   ```

2. 작업 완료 후 commit

   ```bash
   # README.md 파일 수정
   $ git status
   On branch practice
   '''
           modified:   README.md
   '''
   $ git add .
   $ git commit -m 'Update and Complete'
   ```


3. master브랜치로 이동

   ```bash
   $ git checkout master
   ```


4. *master브랜치에 추가 commit  발생시키기*

   ```bash
   # README.md 파일 수정
   '''
   $ git add .
   $ git commit -m 'Update README'
   ```
   
5. master브랜치에 병합

   ```bash
   $ git merge practice
   Auto-merging README.md
   CONFLICT (content): Merge conflict in README.md
   Automatic merge failed; fix conflicts and then commit the result.
   ```


6. merge conflict 발생

   ```bash
   $ git status
   On branch master
   You have unmerged paths.
     (fix conflicts and run "git commit")
     (use "git merge --abort" to abort the merge)
   
   Unmerged paths:
     (use "git add <file>..." to mark resolution)
           both modified:   README.md
   ```


7. 충돌 확인 및 해결

   충돌이 발생한 파일을 visual studio code로 연다

   ```bash
   <<<<<<< HEAD
   abcdefg
   =======
   bibidibabidibu!
   >>>>>>> practice
   ```
   
   원하는 형태의 코드로 직접 수정한 뒤 저장한다
   
   ```bash
   <<<<<<< HEAD
   blablabla
   bibidibabidibu!
   >>>>>>> practice
   ```


8. merge commit 진행

   ```bash
   $ git commit
   ```

   vim 편집기 화면이 나타난다

   <img width="452" alt="gitbasic-5" src="https://user-images.githubusercontent.com/72610879/103472365-0ce80280-4dd0-11eb-8cb4-a1e334d49248.PNG">
   
   자동으로 작성된 커밋 메시지를 확인하고, `esc`와  `:wq`를 차례로 눌러 저장 및 종료한다

   * `w` : write
   * `q` : quit

9. 그래프로 결과 확인하기

   ```bash
   $ git log --oneline --graph
   *   1a08480 (HEAD -> master) Merge branch 'practice'
   |\
   | * 156b027 (practice) Update and Complete
   * | 30c71d2 Update README
   |/
   *   44515f8 Merge branch 'example'
   |\
   | * 6b0245e Complete data
   * | 6930e34 Hotfix
   |/
   * 5ff4709 Complete test
   * c6f5db0 Add README
   ```


10. branch 삭제

    ```bash
    $ git branch -d practice
    Deleted branch practice (was 6b0245e).
    ```

## 기타 명령어

### 1. touch

파일을  생성한다

```bash
$ touch sources.txt
$ touch README.md
```

### 2. cd

폴더로 이동한다

```bash
Hwayeon Kim@DESKTOP-J281U8B MINGW64 ~/Desktop/TIL (master)
$ cd md-images

Hwayeon Kim@DESKTOP-J281U8B MINGW64 ~/Desktop/TIL/md-images (master)
```



*github관련 추가내용은 https://backlog.com/git-tutorial/kr/intro/intro1_1.html 참조*