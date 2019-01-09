
~~~bash
git branch release;
git branch dev;
git branch dev/phase;
git branch dev/func;



git checkout dev/phase;
git commit; git commit; git commit;

git checkout dev/func;
git commit; git commit;


git checkout dev;
git merge dev/phase;
git merge dev/func;



git checkout dev/phase;
git commit; git commit; git commit;

git checkout dev/func;
git commit; git commit;


git checkout dev;
git merge dev/phase;
git merge dev/func;





git checkout release;
git merge dev;




git checkout dev/phase;
git commit; git commit; git commit;

git checkout dev/func;
git commit; git commit;


git checkout dev;
git merge dev/phase;
git merge dev/func;


git checkout release;
git merge dev;



git checkout master;
git merge release;
git tag v1.0.0;


git checkout -b hotfix;
git commit; git commit;
git checkout master;
git merge hotfix;
git branch -d hotfix;



git checkout dev/phase;
git commit; git commit; git commit; git commit;

git checkout dev/func;
git commit; git commit; git commit;


git checkout dev;
git merge dev/phase;
git merge dev/func;


git checkout release;
git commit; git commit; git commit; git commit; git commit;
git checkout master;
git merge release;

git checkout dev;
git merge release;

~~~