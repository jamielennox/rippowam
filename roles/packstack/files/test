import pprint
import webob
import webob.dec


@webob.dec.wsgify
def application(req):
    return webob.Response(pprint.pformat(req.environ),
                          content_type='application/json')
