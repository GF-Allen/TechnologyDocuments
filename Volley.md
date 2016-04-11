### Volley

1. 定义一个全局的RequestQueue，用于缓存所有的Http请求，一般可以放在Application

		RequestQueue mRequestQueue  = Volley.newRequestQueue(context);

2. 然后new一个请求，这里用JsonObjectRequest（JsonArrayRequest同理）

		JsonObjectRequest jsonObjectRequest = new JsonObjectRequest(url, null,  
        new Response.Listener<JSONObject>() {  
            @Override  
            public void onResponse(JSONObject response) {  
                Log.d("mTAG", response.toString());  
            }  
        }, new Response.ErrorListener() {  
            @Override  
            public void onErrorResponse(VolleyError error) {  
                Log.e("mTAG", error.getMessage(), error);  
            }  
        });

3. 将请求添加到requestQueue中即可

	mRequestQueue.add(jsonObjectRequest);

> 自定义Request

1. 自定义GsonRequest解析json

		public class GsonRequest<T> extends Request<T> {

		    private final Listener<T> mListener;
		    private static Gson mGson = new Gson();
		    private Class<T> mClass;
		    private Map<String, String> mParams;//post Params
		    private TypeToken<T> mTypeToken;
		
		
		    public GsonRequest(int method, Map<String, String> params, String url, Class<T> clazz, Listener<T> listener,
		                       ErrorListener errorListener) {
		        super(method, url, errorListener);
		        mClass = clazz;
		        mListener = listener;
		        mParams = params;
		    }
		
		
		    public GsonRequest(int method, Map<String, String> params, String url, TypeToken<T> typeToken, Listener<T> listener,
		                       ErrorListener errorListener) {
		        super(method, url, errorListener);
		        mTypeToken = typeToken;
		        mListener = listener;
		        mParams = params;
		    }
		
		    //get
		    public GsonRequest(String url, Class<T> clazz, Listener<T> listener, ErrorListener errorListener) {
		        this(Method.GET, null, url, clazz, listener, errorListener);
		    }
		
		    public GsonRequest(String url, TypeToken<T> typeToken, Listener<T> listener, ErrorListener errorListener) {
		
		        this(Method.GET, null, url, typeToken, listener, errorListener);
		
		    }
		
		    @Override
		    protected Map<String, String> getParams() throws AuthFailureError {
		        return mParams;
		    }
		
		    @Override
		    protected Response<T> parseNetworkResponse(NetworkResponse response) {
		        try {
		            String jsonString = new String(response.data, HttpHeaderParser.parseCharset(response.headers));
		            if (mTypeToken == null)
		                return Response.success(mGson.fromJson(jsonString, mClass),
		                        HttpHeaderParser.parseCacheHeaders(response));//用Gson解析返回Java对象
		            else
		                return (Response<T>) Response.success(mGson.fromJson(jsonString, mTypeToken.getType()),
		                        HttpHeaderParser.parseCacheHeaders(response));//通过构造TypeToken让Gson解析成自定义的对象类型
		
		        } catch (UnsupportedEncodingException e) {
		            return Response.error(new ParseError(e));
		        }
		    }
		
		    @Override
		    protected void deliverResponse(T response) {
		        mListener.onResponse(response);
		    }
		}

2. 如果传输大文件要使用Jackson库，JacksonRequest的定义

		public class JacksonRequest<T> extends Request<T> {
		
		    private final Listener<T> mListener;
		
		    private static ObjectMapper objectMapper = new ObjectMapper();
		
		    private Class<T> mClass;
		
		    private TypeReference<T> mTypeReference;//提供解析复杂JSON数据支持
		
		    public JacksonRequest(int method, String url, Class<T> clazz, Listener<T> listener,
		                          ErrorListener errorListener) {
		        super(method, url, errorListener);
		        mClass = clazz;
		        mListener = listener;
		    }
		
		    public JacksonRequest(int method, String url, TypeReference<T> typeReference, Listener<T> listener,
		                          ErrorListener errorListener) {
		        super(method, url, errorListener);
		        mTypeReference = typeReference;
		        mListener = listener;
		    }
		
		    public JacksonRequest(String url, Class<T> clazz, Listener<T> listener, ErrorListener errorListener) {
		        this(Method.GET, url, clazz, listener, errorListener);
		    }
		
		    public JacksonRequest(String url, TypeReference<T> typeReference, Listener<T> listener,
		                          ErrorListener errorListener) {
		        super(Method.GET, url, errorListener);
		        mTypeReference = typeReference;
		        mListener = listener;
		    }
		
		    @Override
		    protected Response<T> parseNetworkResponse(NetworkResponse response) {
		        try {
		            String jsonString = new String(response.data, HttpHeaderParser.parseCharset(response.headers));
		            Log.v("mTAG", "json");
		            if (mTypeReference == null)//使用Jackson默认的方式解析到mClass类对象
		
		                return (Response<T>) Response.success(
		                        objectMapper.readValue(jsonString, TypeFactory.rawClass(mClass)),
		                        HttpHeaderParser.parseCacheHeaders(response));
		            else//通过构造TypeReference让Jackson解析成自定义的对象类型
		                return (Response<T>) Response.success(objectMapper.readValue(jsonString, mTypeReference),
		                        HttpHeaderParser.parseCacheHeaders(response));
		        } catch (Exception e) {
		            return Response.error(new ParseError(e));
		        }
		    }
		
		    @Override
		    protected void deliverResponse(T response) {
		        mListener.onResponse(response);
		    }
		
		}

3. 加载图片

	1. volley有加载网络图片的功能，可以new一个ImageRequest，不过它并没有缓存处理，所以我们用ImageLoader（volley.toolbox.ImageLoader），volley内部实现了磁盘缓存，不过没有内存缓存，我们可以自己定义。 1.新建一个ImageLoader，设置ImageListener，然后在get方法中传入url

			ImageLoader imageLoader = new ImageLoader(mRequestQueue, new MyImageCache());
			ImageLoader.ImageListener listener = ImageLoader.getImageListener(mImageview,R.mipmap.ic_default, R.mipmap.ic_error);
			imageLoader.get("https://d262ilb51hltx0.cloudfront.net/max/800/1*dWGwx6UUjc0tocYzFNBLEw.jpeg",listener, 800, 800);

	2. 为了实现图片的内存缓存，我们使用LruCache来实现，自定义一个MyImageCache类继承自ImageCache，在其构造方法中new一个最大为8M的LruCache

			public class MyImageCache implements ImageLoader.ImageCache {

			    private LruCache<String, Bitmap> mCache;
			
			    public MyImageCache() {
			        int maxSize = 8 * 1024 * 1024;
			        mCache = new LruCache<String, Bitmap>(maxSize) {
			            @Override
			            protected int sizeOf(String key, Bitmap bitmap) {
			               //getRowBytes()返回图片每行的字节数，乘以高度得到图片的size
			                return bitmap.getRowBytes() * bitmap.getHeight();
			            }
			        };
			    }    
			    @Override
			    public Bitmap getBitmap(String url) {
			        return mCache.get(url);
			    }
			
			    @Override
			    public void putBitmap(String url, Bitmap bitmap) {
			        mCache.put(url, bitmap);
			    }    
			}