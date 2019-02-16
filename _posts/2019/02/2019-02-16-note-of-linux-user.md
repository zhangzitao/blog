# group
### create group
add a 'test' group
    
    groupadd test
### modify group
change 'test' group's name to 'test2'
    
    groupmod -n test2  test
### delete group
delete 'test2'
    
    groupdel test2
    
### check group
check current user's group
    
    groups
check 'test' user's group
    
    groups test
check all groups

    cat /etc/group

# user
### create user
add a 'test' user
    
    useradd test
    passwd test
### modify user
change 'test' user's root path
    
    usermod -d /home/test test
add and delete 'test' user to 'test2'
    
    usermod -a -G test test2
    usermod -d -G test test2
### delete user
delete 'test2'
    
    userdel test2
### check user

    id test
