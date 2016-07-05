---
layout: post
title:  "[OpenStack] Horizon Local Development Environment"
date:   2016-07-05
categories: openstack
tags:
- openstack
- horizon
---

* content
{:toc}

## Overview
`OpenStack` 프로젝트 중 `Horizon`은 `OpenStack`의 대시보드 서비스이다.
지금 근무하고 있는 회사에서 `OpenStack`을 이용하여 퍼블릭 클라우드 서비스를 준비 중에 있는데 이 `Horizon`을 토대로 사용자 콘솔을 재구성하기로 의견이 모아져서 오늘은 로컬 환경에 `Horizon` 개발 환경을 구축하는 방법을 포스팅한다.
기준이 되는 버전은 `OpenStack Liberty`버전이고 개발환경은 `OSX`이다.

## Clone git repository
`Horizon`의 개발환경을 셋팅하는 방법은 간단하다. `Git`에서 소스코드를 클론 받고 일부 설정만 변경해준 후 서버에 올리면 끝이다.
그 중 첫번째로 아래와 같이 `Github`에서 소스코드를 클론 받는다.

{% highlight bash %}
$ git clone -b stable/liberty https://github.com/openstack/horizon.git
{% endhighlight %}

소스코드 다운로드가 완료되면 아래와 같이 `run_tests.sh` 스크립트를 실행시킨다.

{% highlight bash %}
$ cd horizon
$ ./run_tests.sh
{% endhighlight %}

위의 스크립트를 실행시키면 먼저 `virtualenv`를 빌드해 `Horizon` 동작에 필요한 모든 의존 관계를 설치한다. 이후에는 내부적으로 포함되어있는 `Unit test suites`를 실행시킨다.

  
## Endpoint Setting

위의 단계를 모두 성공적으로 마쳤다면 이제 `Endpoint`설정 단계만 남았다.
아래의 명령을 통해 `local_settings.py`파일을 생성한다. 
`local_settings.py`파일에는 `Horizon`을 동작하는데 필요한 설정정보가 들어있다.

{% highlight bash %}
$ cp openstack_dashboard/local/local_settings.py.example openstack_dashboard/local/local_settings.py
{% endhighlight %}

`Horizon`소스코드에 이미 들어있던 예제 파일을 통해 `local_settings.py`파일을 생성한 후 아래와 같이 `Endpoint URL`을 바라보고자 하는 `OpenStack`환경으로 설정하면 끝이다.

{% highlight bash %}
OPENSTACK_HOST = "<YOUR_OWN_OPENSTACK_ENDPOINT_HOST>"
{% endhighlight %}

이제 `Horizon`을 로컬에서 동작하기 위한 모든 준비를 마쳤다.

## Start the Horizon Development Server

아래의 명령어를 통해 `Horizon`을 동작시킨다.

{% highlight bash %}
$ ./run_tests.sh --runserver localhost:9000
{% endhighlight %}
