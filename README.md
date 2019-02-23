# Code that obtains VK tokens that work for VK audio API

Read this in [russian](README.ru.md).

This code makes it possible to obtain VK token, that works for VK audio API, so you can query audio URIs. The code was obtained through reversing Kate Mobile application and official VK client. VK login, VK password, GMS ID and GMS token are needed. (The last two only for Kate Mobile based method) No additional dependencies are required.

See files in the `examples` directory to see how it can be used. These scripts obtain both GMS credentials and VK token. Just run one of them: `example_microg.php login pass` and it will print the token. Examples that show how to use obtained VK token are in the `usage` subdirectory.

There is also more advanced CLI tool, that emulates Kate Mobile:
```
Usage: src/cli/vk-audio-token.php [options] vk_login vk_pass
Options:
-s file             - save GMS ID and token to the file
-l file             - load GMS ID and token from file
-g gms_id:gms_token - use specified GMS ID and token
-d file             - use droidguard string from file
                      instead of hardcoded one
-m                  - make microG checkin
                      by default checkin
                      with droidguard string is made
-h                  - print this help
```

## Docker
```
docker build -t vk-audio-tokens src/
docker run -t vk-audio-tokens:latest php src/cli/vk-audio-token.php -m vk_login vk_pass
docker run -t vk-audio-tokens:latest php src/examples/usage/example_kate.php token
```

## GMS Credentials

There are two ways to obtain GMS credentials.

The first way is to get them from a rooted Android device. The token is in `/data/data/com.google.android.gsf/shared_prefs/CheckinService.xml` file and ID is in `/data/data/com.google.android.gms/shared_prefs/Checkin.xml` file. You can install [GMS Credentials](https://github.com/vodka2/gms-credentials) application to see them.

The second way is to perform Android Checkin yourself. Class `AndroidCheckin` is designed for this task. This class provides two options: checkin with droidguard string and checkin as in [microG](https://github.com/microg) project. Note that the obtained credentials may expire.

For the first option you need a string that is generated by `com.google.ccc.abuse.droidguard` (the.apk). One such string is in `example_droidguard_str.php` file, it may expire. When using the second option one extra request is made and PHP needs to have sockets enabled.

It is also possible to intercept Android Checkin request (it is made on first boot) and see the returned GMS ID and token.