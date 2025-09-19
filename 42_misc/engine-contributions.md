use Array when it has to be passed to exposed API (e.g. as a return value of a method)
use Vector when the data is passed around internally
use LocalVector when the data is local to instance and not used as method argument etc.
List is generally discouraged nowadays in favor of Vector/LocalVector. Normally it should be used when element count changes frequently (e.g. due to repeated push_back())


