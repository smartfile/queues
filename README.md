# queues
A lowest-common-denominator API for interacting with lightweight queue services.

This is a very simple API for interacting with lightweight queues. Currently queues that speak the memcached protocol, Amazon's SQS service, and Beanstalkd are supported. External backends can be easily defined and used as long as they conform to the BaseQueue API.

This library is designed to be configured either from a Django settings file or via environment variables. Heavy lifting is done by one of the backend libraries:

|   Backend   |                    Library                    | License |
|:-----------:|:---------------------------------------------:|:-------:|
|     sqs     |                      boto                     |   MIT   |
|  memcache*  |                python-memcached               |   PSF   |
|             |                    cmemcache                  |   GPL   |
| beanstalkd  |                   beanstalkc                  | APL-2.0 |
|    redisd   |                     redis                     |   MIT   |
|  zenqueued  |                    zenqueue                   |   MIT   |
|   dummy     | built-in (useful for single-threaded testing) |   MIT   |

* Memcached is not supported for queueing. You must use a queue server that is compatible with the memcached protocol such as MemcacheQ etc.


###Installation
```
pip install git+https://github.com/smartfile/queues.git#egg=queues
```

###Example usage:

```
$ export QUEUE_BACKEND=memcached
$ export QUEUE_MEMCACHE_CONNECTION=localhost:21122
$ python
>>> from queues import queues
>>> q = queues.Queue('myname')
>>> q.write('test')
True
>>> len(q)
1
>>> q.read()
test
>>> queues.get_list()
['myname']
```

Any comments or feedback would be appreciated. Please note that the API may change in future versions based on feedback and use. 
