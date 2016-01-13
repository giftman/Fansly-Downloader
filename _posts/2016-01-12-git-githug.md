
#githug

--
reset,rebase 
reset 恢复到某一状态,保留修改待提交或者全部舍弃前面的修改  
rebase 以某个版本为基础重新在它的基础上重新提交修改内容/以某个版本为基础交互修改log,sqush,reorder

reset,chekcout
reset 以仓库内容覆盖
checkout 以Staging area 内容覆盖

HEAD HEAD^ HEAD^1
git push origin  master
git push origin  test_branch


其它实践再补充

--

####Level 1: init 
`
$ git init
`

####Level 2: config 
`
$ git config --global user.name "winfan"
$ git config --global user.email "winfan@123.com"
`

####Level 3: add 
`
$ git add README
`

####Level 4: commit 
`
$ git commit -m "add R.. commit"
`

####Level 5: clone 
`
$ git clone https://github.com/Gazler/cloneme
`

####Level 6: clone_to_folder 
`
$ git clone https://github.com/Gazler/cloneme
 my_cloned_repo
`
####Level 7: ignore 
`
$ vim .gitignore # Add line "*.swp"
`
####Level 8: include 
`
$ vim .gitignore # Add line "*.a !lib.a"
`

####Level 9: status 
`
$ git status
`
####Level 10: number_of_files_committed 
`
$ git status
`

####Level 11: rm 
`
git rm deleteme.rb
`
####Level 12: rm_cached //start  remove from staging area
`
$ git rm --cached deleteme.rb
`
####Level 13: stash //git stash -h
`
$ git stash
`
####Level 14: rename 
`
$ git mv oldfile.txt newfile.txt
`
####Level 15: restructure 
`
mkdir src
$ git mv *.html src
`
####Level 16: log 
`
$ git log
`
####Level 17: tag 
`
$ git tag new_tag
`
####Level 18: push_tags //TODO
`
$ git push --tags origin master
`
####Level 19: commit_amend  // add,change the latest commit message 
`
$ git add forgotten_file.rb
$ git commit --amend
`
####Level 20: commit_in_future //my answer is change the computer time...
`
$ git commit -m "future" --date="Sun Jan 3 13:40:06 2017 +0800"
`
####Level 21: reset //reset checkout

git reset 恢复到某个版本状态，--soft用仓库的内容覆盖staging area 和working directory,如果staging area和working directory有你自己的内容，你有新的修改没提交或者提交了没commit,就保留你的。--hard就不用想这么多，直接全部内容仓库的。
git checkout 则是使用 staging area 的中的版本覆盖 working directory。

`
$ git reset HEAD
$ git add to_commit_first.rb
`
####Level 22: reset_soft //start

1 --soft 参数将上一次的修改放入 staging area
2 --mixed 参数将上一次的修改放入 working directory
3 --hard 参数直接将上一次的修改抛弃
`
$ git reset --soft HEAD^
`
####Level 23: checkout_file 
`
$ git checkout config.rb
`
####Level 24: remote 
`
$ git remote -v
`
####Level 25: remote_url 
`
$ git remote -v
`
####Level 26: pull  //start
`
$ git pull origin master
`
####Level 27: remote_add 
`
$ git remote add origin https://github.com/githug/githug
`
####Level 28: push  //TODO start
`
$ git rebase origin/master master
$ git push origin master
`
####Level 29: diff 
`
$ git diff HEAD
`
####Level 30: blame 
`
$ git blame config.rb
`

####Level 35: branch_at //TODO start

`
$git branch test_branch HEAD^1
`
####Level 36: delete_branch 
`
$ git branch -d delete_me
`
####Level 37: push_branch //
`
$ git push origin test_branch
`
####Level 38: merge 
`
$ git merge feature master
`
####Level 39: fetch //== git fetch && git merge
`
$ git fetch origin
`
####Level 40: rebase 
在使用此命令的时候，需要非常注意的是，不要 rebase 哪些已经推送到公共库的更新，因为此操作是重新应用修改，所以公共库的更新可能已经被其他协作者所同步，如果再次 rebase 这些修改，将可能zh

`
$ git rebase master feature
`
####Level 41: repack 
`
$ git repack -a -d
`
####Level 42: cherry-pick 
`
$ git checkout master
$ git cherry-pick new-feature~2
`
####Level 43: grep 
`
$ git grep TODO
`
####Level 44: rename_commit 
`
$ git rebase -i logindex
`
####Level 45: squash 
`
$ git rebase -i log_index
`
####Level 46: merge_squash 
`
$ git merge --squash long-feature-branch
$ git add .
$ git commit -m "Merge branch long-feature-branch"
`
####Level 47: reorder 
`
$ git rebase -i log_index
`
####Level 48: bisect 

`
$ git bisect start
$ git bisect bad
$ git bisect good [first commit]
$ git bisect run ruby prog.rb 5    # 二分查找直至找出 bad 提交 

$ git bisect start master f608824888b83
`
####Level 49: stage_lines 
`
$ git add -p feature.rb # 编辑提交指定行 
or
$ git add -e feature.rb
`

####Level 50: find_old_branch 

`
$ git reflog
$ git checkout solve_world_hunger
`
####Level 51: revert 
`
$ git revert HEAD^1
`
####Level 52: restore 
`
$ git reflog
$ git rebase --log_index
or git checkout?
`
####Level 53: conflict 

`
$ git merge mybranch
`
resolve conflicts and finish the merge
`
$ git add .
$ git commit -m "Fix merge"
`
####Level 54: submodule 
`
$ git submodule add https://github.com/jackmaney/githug-include-me githug-include-merge
`
####Level 55: contribute 
`
:)
Githug is designed to give you a practical way of learning git. It has a series of levels, each req


ref:
http://www.jianshu.com/p/482b32716bbe
http://blog.csdn.net/trochiluses/article/details/8996431
