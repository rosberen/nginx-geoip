Description
===========

# nginx-geoip (Rules for the geoip2 module)


Building nginx with the ngx_http_geoip2_module module [https://github.com/leev/ngx_http_geoip2_module]

**ngx_http_geoip2_module** - creates variables with values from the maxmind geoip2 databases based on the client IP (default) or from a specific variable (supports both IPv4 and IPv6)

The module now supports nginx streams and can be used in the same way the http module can be used.

## Installing
First install [libmaxminddb](https://github.com/maxmind/libmaxminddb) as described in its [README.md
file](https://github.com/maxmind/libmaxminddb/blob/main/README.md#installing-from-a-tarball).

#### Download nginx source
```
wget http://nginx.org/download/nginx-VERSION.tar.gz
tar zxvf nginx-VERSION.tar.gz
cd nginx-VERSION
```

##### To build as a dynamic module (nginx 1.9.11+):
```
./configure --add-dynamic-module=/path/to/ngx_http_geoip2_module
make
make install
```

##### In nginx.conf, you need to add to the http directive:

```
geoip2 /etc/nginx/GeoIP/GeoIP2-City.mmdb {
        $geoip2_data_country_iso_code country iso_code;
        $geoip2_data_city_name city names en;
            }

    map $geoip2_data_city_name $geo_city_perm {
        Amsterdam yes;
            }            
    map $geoip2_data_country_iso_code $geo_country_perm {
        USA yes; # USA
            }
    geo $allow_addresses {
        include /etc/nginx/GeoIP/whitelist.conf;
            }
```


Rules will be taken into account for GeoIP:
  - Data from the maxmind database: Country, City
  - Whitelist under networks