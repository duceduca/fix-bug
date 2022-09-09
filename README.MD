16/05/2022

Release tutor id
-
- B1 : set env ( thay lại cho đúng giá trị)

```php
TUTOR_ID_CLIENT_ID=
TUTOR_ID_CLIENT_SECRET=
TUTOR_ID_REDIRECT=domain.com.vn/auth/id_educa_tutor/callback

TUTOR_ID_URL_AUTHORIZE=http://lms-test.edupiatutor.vn/realms/Backend/protocol/openid-connect/auth
TUTOR_ID_URL_GET_TOKEN=http://lms-test.edupiatutor.vn/realms/Backend/protocol/openid-connect/token
TUTOR_ID_URL_GET_USER_BY_TOKEN=http://lms-test.edupiatutor.vn/realms/Backend/protocol/openid-connect/userinfo
TUTOR_ID_URL_LOGOUT=http://lms-test.edupiatutor.vn/realms/Backend/protocol/openid-connect/logout
```

- B2 : Update composer ( để install tutor/id )
- B3 : Merge
  https://gitlab.edupiakid.vn/educa-tutor/hrm/merge_requests/1
- B4 : Chạy
  php artisan migrate --path=database/migrations/2022_05_11_100326_add_tutor_id_to_users_table_and_tutor_role_to_roles_table.php (e


[comment]: <> (Release create employee)

[comment]: <> (- )

[comment]: <> (- B1 : Merge https://gitlab.edupiakid.vn/educa-tutor/hrm/merge_requests/2)

[comment]: <> (- B2 : Chạy php artisan migrate --path=database/migrations/2022_05_16_022621_add_tutor_employee_profile_to_profiles_table.php)

[comment]: <> (- B3 : Chạy : php artisan db:seed --class=AddPermissionCreateEmployee)

[comment]: <> (23/5/2022)

[comment]: <> (Release create employee từ excel)

[comment]: <> (-)

[comment]: <> (- B1 : Merge : https://gitlab.edupiakid.vn/educa-tutor/hrm/merge_requests/4)

[comment]: <> (- B2 : Chạy : php artisan migrate --path=database/migrations/2022_05_16_073312_add_unique_employee_id_to_profiles_table.php)

[comment]: <> (- B3 : Set env TUTOR_CLIENT_SECRET)



[comment]: <> (1/6/2022)

[comment]: <> (API Profile Employee + Swagger)

[comment]: <> (- Ở LAB : )

[comment]: <> (  - Merge code :)

[comment]: <> (  - Chạy : php artisan migrate --path=database/migrations/2022_05_31_145528_delete_tutor_profile_in_profiles_table.php)

[comment]: <> (  - Chạy : php artisan migrate --path=database/migrations/2022_05_27_162757_create_tutor_profiles_table.php)

[comment]: <> (  - Chạy : php artisan db:seed --class=DemoDataTutorProfiles)

[comment]: <> (- Ở Product :)

[comment]: <> (  - Chạy : php artisan migrate --path=database/migrations/2022_05_16_073312_add_unique_employee_id_to_profiles_table.php)

[comment]: <> (  - Chạy : php artisan migrate --path=database/migrations/2022_05_27_162757_create_tutor_profiles_table.php)


16/6
Event create employee
- B1 : Merge code 
- B2 : Setup redis nếu chưa có
- B3 : set ENV QUEUE_CONNECTION=redis
- B2 : setup supervisor chạy :
  - php artisan queue:work --queue=create-tutor-id-user --delay=60

29/6
Import employee role
- B1 : Chạy php artisan migrate
- Setup supervisor chạy :
    - php artisan queue:work --queue=add-service-role-for-employee --delay=60

15/7
Calculate payroll (lương buổi dạy)
- B1 : Chạy php artisan migrate
- Set env:
    - TUTOR_LMS_URL
    - TUTOR_LMS_CLIENT_SECRET
    - TUTOR_TICKET_URL
    - TUTOR_TICKET_CLIENT_SECRET
- Chạy php artisan db:seed --class=AddPermission

25/7
API tạo user role employee cho lms
- Set env:
    - CREATE_EMPLOYEE_DEFAULT_DEPARTMENT_ID=
    - CREATE_EMPLOYEE_DEFAULT_DESIGNATION_ID=
    - CREATE_EMPLOYEE_DEFAULT_EMPLOYMENT_STATUS_ID=

28/7
Thêm middleware auth server to server bằng client-secret
- Set env:
    - CLIENT_SECRET_API_AUTHENTICATION=


5/8
Update tutor id khi hrm update
setup supervisor chạy :
- php artisan queue:work --queue=update-tutor-id-user --delay=10

8/8
Tự generate employee_id
Chạy : 
php artisan migrate --path=database/migrations/2022_08_08_092021_add_not_null_name_to_users_table.php

9/8
Update tutor id profile khi hrm update
setup supervisor chạy :
- php artisan queue:work --queue=update-tutor-id-profile --delay=10

12/8 
Thêm profile gia sư , nhân viên
Chạy 
php artisan db:seed --class=AddProfileGiaSu
php artisan db:seed --class=AddProfileNhanVien

29/8
Thêm profile cộng tác viện
Chạy
php artisan db:seed --class=AddProfileCongTacVien

6/9/2022
Thêm relation ship attribute profile
Chạy : php artisan migrate --path=database/migrations/2022_09_06_134659_add_parent_id_to_tutor_profile_attributes_table.php

7/9/2022
run to sync attendance_record data to timesheet
$ : php artisan timesheet:sync --start=20220801 --end=20220810 
if option null, --start, --end = today yyyymmdd# fix-bug
