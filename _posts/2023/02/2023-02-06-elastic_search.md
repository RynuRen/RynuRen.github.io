---
layout: jupyter
title: Elastic Search
published: true
date: 2023-02-06
description: Elastic Search
comments: true
categories:
 - elastic_search
tags:
 - elastic_search
toc: true
toc_sticky: true
toc_label: Elastic Search
use_math: true
---
---
# Elastic Search

## 개요

* ELK stack (Elastic search, Log stash, Kibana +Beats)

(https://www.elastic.co/kr/)

검색 기술, DB 기술, 자연어 처리 등 여러 영역에 포함될 수 있다.  
한마디로 유연한 검색 기능

* 파이썬에서 찾기

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
str1 = "This final class with you"
str1.find("class")
```

</div>

<div class="output_prompt">
Out&nbsp;[1]:
</div>




{:.output_data_text}

```
11
```



<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
str1.find("cless")
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>




{:.output_data_text}

```
-1
```



정확하게 그 단어가 있어야 검색이 가능하다.

* SQL에서 찾기

|col1|
|-|
|kim|
|lee|
|park (bark, pack)|

박씨에 park, bark, pack 여러 경우가 있다.

SELECT * FROM ~~ WHERE col1='park' => park만 찾을 수 있다.  
SELECT * FROM ~~ WHERE col1 like 'pa%' => park, pack는 찾을 수 있지만 bark은 찾을 수 없다.

> park와 bark, pack의 데이터를 모두 검색해 하나로 merge하면 elastic search를 꼭 할 필요는 없지만 알아두면 좋다.

* aws elasticsearch  
2010년 오픈 소스로 릴리즈 되었으나 2021년 1월 21일 Elastic NV는 라이선스 전략을 변경하여 오픈 소스가 아니게 되었다.  
AWS는 오픈 소스 Elasticsearch 및 Kibana의 ALv2 라이선스 갈래인 OpenSearch 프로젝트를 도입했다.  
(elastic이 amazon과 분쟁)  
(https://aws.amazon.com/ko/what-is/elasticsearch/)

이전 버전의 elasticsearch, kibana를 사용해 연습 (kibana는 es를 컨트롤하는 cmd로 사용)  
(https://www.elastic.co/kr/downloads/past-releases/elasticsearch-7-16-3)  
(https://www.elastic.co/kr/downloads/past-releases/kibana-7-16-3)

* 데이터베이스(DB)
    * RDBMS: 관계형 데이터베이스, MySQL, Maria DB, Oracle DB (table기반), csv - 거의 대부분의 DB  
        bulky한 작업에 유리하다.
    * ObjDB: NoSQL, 구조화 되지 않음, Mongo DB (table없이), json
    * Elastic search: 자기만의 저장방식이 있음 (RDBMS, ObjDB 둘다 아님)

* ELK stack  
데이터 흐름에 따라
    * Beats: 엣지 데이터 수집
    * Log stash: 분산형 데이터 수집(서버 로그 수집 변환)
    * Elastic sear접속화면ch: 저장 & 검색
    * Kibana: 시각화(정기적으로 분석)

> 일반적인 DB와 다르게 Dashboard에 최적화되어 있다.

## elastic search 기본 사용

elastic search, bin 폴더 내의 elasticsearch.bat을 cmd 창에 실행하고  
새 cmd 창에는 kibana, bin 폴더 내의 kibana.bat파일을 실행한다.

* elastic search 포트 9200
* kibana 포트 5601

* http://localhost:9200/ 접속화면

![img]({{ '/assets/images/2023/02/06/img02.png' | relative_url }})

* http://localhost:5601/ 접속화면

![img]({{ '/assets/images/2023/02/06/img01.png' | relative_url }})

* console창 진입

![img]({{ '/assets/images/2023/02/06/img03.png' | relative_url }})

![img]({{ '/assets/images/2023/02/06/img04.png' | relative_url }})

* DB 확인

>Input
{:.filename}
{% highlight json %}
GET _cat/indices
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.16/security-minimal-setup.html to enable security.
green open .geoip_databases                nu9cOPplTXGRZW97XzOC6Q 1 0 41   36 42.3mb 42.3mb
green open .kibana_7.16.3_001              ckefQ50JST-hcN1NfHB4iw 1 0 39    0  2.3mb  2.3mb
green open .apm-custom-link                iMVH1OCOT1yYti39KKghuA 1 0  0    0   226b   226b
green open .apm-agent-configuration        vfzY1dkmRt2cNlVSWjry3A 1 0  0    0   226b   226b
green open .kibana_task_manager_7.16.3_001 bU4Y5PqtQZywlEc8rkf0jA 1 0 17 4379  2.2mb  2.2mb
green open .tasks                          YXL-55Z6SV2CIH9SlZZtHQ 1 0  2    0 13.8kb 13.8kb
{% endhighlight %}

### PUT 명령

>Input
{:.filename}
{% highlight json %}
PUT index1
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "index1"
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
PUT index2/_doc/1
{
  "name":"mike",
  "age":25,
  "gender":"male"
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
{% endhighlight %}

### GET 명령

>Input
{:.filename}
{% highlight json %}
GET index1
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "index1" : {
    "aliases" : { },
    "mappings" : { },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "index1",
        "creation_date" : "1677400022091",
        "number_of_replicas" : "1",
        "uuid" : "PUrwUD3mT6GHeUtlL9WlCg",
        "version" : {
          "created" : "7160399"
        }
      }
    }
  }
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET index2
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "index2" : {
    "aliases" : { },
    "mappings" : {
      "properties" : {
        "age" : {
          "type" : "long"
        },
        "gender" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "name" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "index2",
        "creation_date" : "1677400082042",
        "number_of_replicas" : "1",
        "uuid" : "SHApFafVSy6xaMyrZvebSg",
        "version" : {
          "created" : "7160399"
        }
      }
    }
  }
}
{% endhighlight %}
index2에 \_doc1이 담겨진다는 표현이 맞지만 익숙한 SQL에 빗대어 표현하자면  
* index2: sql의 표  
* \_doc/1: sql의 row  
* properties: sql의 column

>Input
{:.filename}
{% highlight json %}
GET _cat/indices
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.16/security-minimal-setup.html to enable security.
green  open .kibana_7.16.3_001              ckefQ50JST-hcN1NfHB4iw 1 0 39    0  2.3mb  2.3mb
green  open .geoip_databases                nu9cOPplTXGRZW97XzOC6Q 1 0 41   36 42.3mb 42.3mb
green  open .apm-custom-link                iMVH1OCOT1yYti39KKghuA 1 0  0    0   226b   226b
yellow open index1                          Bzax7AxLS1yGVO780Mrmtg 1 1  0    0   226b   226b
green  open .apm-agent-configuration        vfzY1dkmRt2cNlVSWjry3A 1 0  0    0   226b   226b
yellow open index2                          2N7CjDhYSRCwVgrNm2GALg 1 1  1    0   226b   226b
green  open .kibana_task_manager_7.16.3_001 bU4Y5PqtQZywlEc8rkf0jA 1 0 17 4405  2.2mb  2.2mb
green  open .tasks                          YXL-55Z6SV2CIH9SlZZtHQ 1 0  2    0 13.8kb 13.8kb
{% endhighlight %}
index1와 index2가 추가된 것을 확인할 수 있다.

>Input
{:.filename}
{% highlight json %}
PUT index2/_doc/2
{
  "name":"katty",
  "age":24,
  "hobby":"napping"
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "2",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET index2
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "index2" : {
    "aliases" : { },
    "mappings" : {
      "properties" : {
        "age" : {
          "type" : "long"
        },
        "gender" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "hobby" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "name" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "index2",
        "creation_date" : "1677400082042",
        "number_of_replicas" : "1",
        "uuid" : "SHApFafVSy6xaMyrZvebSg",
        "version" : {
          "created" : "7160399"
        }
      }
    }
  }
}
{% endhighlight %}
elastic search의 컬럼(properties)은 데이터 별로 존재할 수 있다.

* 특정 doc GET

>Input
{:.filename}
{% highlight json %}
GET index2/_doc/1
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "mike",
    "age" : 25,
    "gender" : "male"
  }
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET index2/_doc/2
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "2",
  "_version" : 1,
  "_seq_no" : 1,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "katty",
    "age" : 24,
    "hobby" : "napping"
  }
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET index2/_doc/2
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "2",
  "_version" : 1,
  "_seq_no" : 1,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "katty",
    "age" : 24,
    "hobby" : "napping"
  }
}
{% endhighlight %}

* index의 전체 doc GET

>Input
{:.filename}
{% highlight json %}
GET index2/_search
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "took" : 883,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "index2",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "name" : "mike",
          "age" : 25,
          "gender" : "male"
        }
      },
      {
        "_index" : "index2",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "name" : "katty",
          "age" : 24,
          "hobby" : "napping"
        }
      }
    ]
  }
}
{% endhighlight %}

### DELETE 명령

>Input
{:.filename}
{% highlight json %}
DELETE index1
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "acknowledged" : true
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET _cat/indices
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.16/security-minimal-setup.html to enable security.
green  open .kibana_7.16.3_001              ckefQ50JST-hcN1NfHB4iw 1 0 39    0  2.3mb  2.3mb
green  open .geoip_databases                nu9cOPplTXGRZW97XzOC6Q 1 0 41   36 42.3mb 42.3mb
green  open .apm-custom-link                iMVH1OCOT1yYti39KKghuA 1 0  0    0   226b   226b
green  open .apm-agent-configuration        vfzY1dkmRt2cNlVSWjry3A 1 0  0    0   226b   226b
green  open .kibana_task_manager_7.16.3_001 bU4Y5PqtQZywlEc8rkf0jA 1 0 17 4500  2.2mb  2.2mb
yellow open index2                          2N7CjDhYSRCwVgrNm2GALg 1 1  1    0  4.5kb  4.5kb
green  open .tasks                          YXL-55Z6SV2CIH9SlZZtHQ 1 0  2    0 13.8kb 13.8kb
{% endhighlight %}
index1이 삭제된 것을 확인할 수 있다.

* 특정 doc DELETE

>Input
{:.filename}
{% highlight json %}
DELETE index2/_doc/2
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "2",
  "_version" : 2,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 4,
  "_primary_term" : 1
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET index2/_search
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "took" : 1178,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "index2",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "name" : "lee",
          "age" : 45,
          "gender" : "male"
        }
      }
    ]
  }
}
{% endhighlight %}

### UPDATE 명령

| | 삽입 | 검색 | 수정 | 삭제 | 
|-|-|-|-|-|
| SQL | INSERT | SELECT | UPDATE | DELETE |
| ES | PUT | GET | (X, UPDATE) | DELETE |


document update는 구현이 어렵다/비싸다 => 덮어쓰기 수행  
흩어진 데이터를 찾아서 수정하기보다 이전에 입력한 걸 무효로 하고 새로 입력(분산형 데이터 베이스에 적합-클라우드 AWS)  
(UPDATE가 있긴 있음)

>Input
{:.filename}
{% highlight json %}
PUT index2/_doc/1
{
  "name":"park",
  "age":45,
  "gender":"male"
}

{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 1
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET index2/_doc/1
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 2,
  "_seq_no" : 2,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "park",
    "age" : 45,
    "gender" : "male"
  }
}
{% endhighlight %}
version이 2로 업데이트 되었다.

>Input
{:.filename}
{% highlight json %}
POST index2/_update/1
{
  "doc":{
    "name":"lee"
  }
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 3,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 3,
  "_primary_term" : 1
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET index2/_doc/1
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "index2",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 3,
  "_seq_no" : 3,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "lee",
    "age" : 45,
    "gender" : "male"
  }
}
{% endhighlight %}
update 되긴 하지만 실제 사용에 오래 걸린다!  
(분산형 시스템이라 저장소 탐색에 시간이 걸림)

데이터 업데이트를 자주하는 상황에서는 ES를 사용하지 않는다. (조회가 자유로운 경우 사용, eg.회사 내규)

> * GET : 서버가 사용자의 의도를 GET - 서버가 주체
> * POST : 사용자가 서버의 내용을 바꿈 - 서버가 수동적

## elastic search 심화 사용

### mapping

* 다이나믹 매핑: 자동으로 type을 결정

>Input
{:.filename}
{% highlight json %}
GET index2
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "index2" : {
    "aliases" : { },
    "mappings" : {
      "properties" : {
        "age" : {
          "type" : "long"
        },
        "gender" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "hobby" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "name" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "index2",
        "creation_date" : "1677401392897",
        "number_of_replicas" : "1",
        "uuid" : "2N7CjDhYSRCwVgrNm2GALg",
        "version" : {
          "created" : "7160399"
        }
      }
    }
  }
}
{% endhighlight %}

* 명시적 매핑:   수동으로 type을 결정

>Input
{:.filename}
{% highlight json %}
PUT index3
{
  "mappings":
  {
    "properties":{
      "name":{
        "type":"text"
      },
      "age":{
        "type":"short"
      },
      "gender":{
        "type":"keyword"
      }
    }
  }
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "index3"
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET index3
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "index3" : {
    "aliases" : { },
    "mappings" : {
      "properties" : {
        "age" : {
          "type" : "short"
        },
        "gender" : {
          "type" : "keyword"
        },
        "name" : {
          "type" : "text"
        }
      }
    },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "index3",
        "creation_date" : "1677404265217",
        "number_of_replicas" : "1",
        "uuid" : "BXByvkYCRkylSv0w4Xl5ug",
        "version" : {
          "created" : "7160399"
        }
      }
    }
  }
}
{% endhighlight %}
구조를 직접 지정  
(수정은 어려우므로 처음에 잘 설정해야 함)

### type: text
부분 단어로 검색이 가능함

>Input
{:.filename}
{% highlight json %}
PUT text_index/_doc/1
{
  "contents":"beautiful day"
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "text_index",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
{% endhighlight %}

값이 제대로 들어갔는지 확인

>Input
{:.filename}
{% highlight json %}
GET text_index/_doc/1
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "text_index",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "contents" : "beautiful day"
  }
}
{% endhighlight %}

type이 text로 다이나믹 매핑되었는지 확인

>Input
{:.filename}
{% highlight json %}
GET text_index
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "text_index" : {
    "aliases" : { },
    "mappings" : {
      "properties" : {
        "contents" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "text_index",
        "creation_date" : "1677404484803",
        "number_of_replicas" : "1",
        "uuid" : "ix0fCqRqTPu-FOm9WlMQpA",
        "version" : {
          "created" : "7160399"
        }
      }
    }
  }
}
{% endhighlight %}

부분 단어로 검색

>Input
{:.filename}
{% highlight json %}
GET text_index/_search
{
  "query":{
    "match":{
      "contents":"day"
    }
  }
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "text_index",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "contents" : "beautiful day"
        }
      }
    ]
  }
}
{% endhighlight %}
* \_score : 0.2876821 단어의 유사도로 검색

>Input
{:.filename}
{% highlight json %}
GET text_index/_search
{
  "query":{
    "match":{
      "contents":"Day"
    }
  }
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "text_index",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "contents" : "beautiful day"
        }
      }
    ]
  }
}
{% endhighlight %}
match로 검색하면 유사도를 계산하여 검색을 해준다.

### type: keyword
범주형 데이터, 주로 집계에 사용됨

>Input
{:.filename}
{% highlight json %}
PUT keyword_index
{
  "mappings":{
    "properties":{
      "contents":{
        "type":"keyword"
      }
    }
  }
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "keyword_index"
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
PUT keyword_index/_doc/1
{
  "contents":"beautiful day"
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "_index" : "keyword_index",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
{% endhighlight %}

>Input
{:.filename}
{% highlight json %}
GET keyword_index/_search
{
  "query":{
    "match":{
      "contents":"day"
    }
  }
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 0,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  }
}
{% endhighlight %}
text는 부분 단어의 유사도로 검색이 가능하나 keyword는 불가능하다

>Input
{:.filename}
{% highlight json %}
GET keyword_index/_search
{
  "query":{
    "match":{
      "contents":"beautiful day"
    }
  }
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "keyword_index",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "contents" : "beautiful day"
        }
      }
    ]
  }
}
{% endhighlight %}
keyword는 정확하게 같아야 검색이 가능하다

### 분석기
자연어 처리의 tokenizer  
검색엔진은 index(색인)가 중요!

* elastic search의 역인덱싱

doc1: i love dog  
doc2: the 10 most cute dog  
love, dog, most, cute => index

index table(index화)  
|word|doc no.|
|---|----|
|love|1|
|dog|1, 2|
|most|2|
|cute|2|

query: cute -> doc2  
query: dog -> doc1, doc2

query->index table->doc (indexing의 역방향)

* MySQL index   
조회 속도는 빨라지지만 Update, Insert, Delete의 속도는 저하(table의 index 갱신에 시간이 걸림)

#### stop
불용어를 포함한 tokenizer

>Input
{:.filename}
{% highlight json %}
post _analyze
{
  "analyzer":"stop",
  "text":"the 10 most cute dog"
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "tokens" : [
    {
      "token" : "most",
      "start_offset" : 7,
      "end_offset" : 11,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "cute",
      "start_offset" : 12,
      "end_offset" : 16,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "dog",
      "start_offset" : 17,
      "end_offset" : 20,
      "type" : "word",
      "position" : 3
    }
  ]
}
{% endhighlight %}
#### simple
불용어 처리를 하지 않음(공백, 숫자, 기호)

>Input
{:.filename}
{% highlight json %}
post _analyze
{
  "analyzer":"simple",
  "text":"the 10 most cute dog"
}
{% endhighlight %}

>Output
{:.filename}
{% highlight json %}
{
  "tokens" : [
    {
      "token" : "the",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "most",
      "start_offset" : 7,
      "end_offset" : 11,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "cute",
      "start_offset" : 12,
      "end_offset" : 16,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "dog",
      "start_offset" : 17,
      "end_offset" : 20,
      "type" : "word",
      "position" : 3
    }
  ]
}
{% endhighlight %}