.PHONY: all
srcdir=.
prefix=squid/
sysconfir=squid/etc/
AWK=awk
EXEEXT=
cf_gen_SOURCES = cf_gen.cc
cf_gen_DEPENDENCIES =
HOSTCXX = g++


DEFAULT_HTTP_PORT	= 3128
DEFAULT_ICP_PORT	= 3130
DEFAULT_PREFIX		= $(prefix)
DEFAULT_CONFIG_DIR	= $(sysconfdir)
DEFAULT_CONFIG_FILE	= $(DEFAULT_CONFIG_DIR)/squid.conf
DEFAULT_MIME_TABLE	= $(DEFAULT_CONFIG_DIR)/mime.conf
DEFAULT_DNSSERVER	= $(libexecdir)/`echo dnsserver | sed '$(transform);s/$$/$(EXEEXT)/'`
DEFAULT_SSL_CRTD	= $(libexecdir)/`echo ssl_crtd  | sed '$(transform);s/$$/$(EXEEXT)/'`
DEFAULT_LOG_PREFIX	= $(DEFAULT_LOG_DIR)
DEFAULT_CACHE_LOG	= $(DEFAULT_LOG_PREFIX)/cache.log
DEFAULT_ACCESS_LOG	= $(DEFAULT_LOG_PREFIX)/access.log
DEFAULT_STORE_LOG	= $(DEFAULT_LOG_PREFIX)/store.log
DEFAULT_NETDB_FILE	= $(DEFAULT_LOG_PREFIX)/netdb.state
DEFAULT_SSL_DB_DIR	= $(localstatedir)/lib/ssl_db
DEFAULT_PINGER		= $(libexecdir)/`echo pinger | sed '$(transform);s/$$/$(EXEEXT)/'`
DEFAULT_UNLINKD		= $(libexecdir)/`echo unlinkd | sed '$(transform);s/$$/$(EXEEXT)/'`
DEFAULT_LOGFILED	= $(libexecdir)/`echo log_file_daemon | sed '$(transform);s/$$/$(EXEEXT)/'`
DEFAULT_DISKD		= $(libexecdir)/`echo diskd | sed '$(transform);s/$$/$(EXEEXT)/'`
DEFAULT_ICON_DIR	= $(datadir)/icons
DEFAULT_ERROR_DIR	= $(datadir)/errors


# cf_gen builds the configuration files.
cf_gen$(EXEEXT): $(cf_gen_SOURCES) $(cf_gen_DEPENDENCIES) cf_gen_defines.cci
	$(HOSTCXX) -o $@ $(srcdir)/cf_gen.cc -I$(srcdir) 

# squid.conf.default is built by cf_gen when making cf_parser.cci
squid.conf.default squid.conf.documented: cf_parser.cci
	true

cf_parser.cci: cf.data cf_gen$(EXEEXT)
	./cf_gen$(EXEEXT) cf.data $(srcdir)/cf.data.depend

# The cf_gen_defines.cci is auto-generated and does not exist when the 
# dependencies computed. We need to add its include files (autoconf.h) here
cf_gen_defines.cci: $(srcdir)/cf_gen_defines $(srcdir)/cf.data.pre
	$(AWK) -f $(srcdir)/cf_gen_defines <$(srcdir)/cf.data.pre >$@ || ($(RM) -f $@ && exit 1)

cf.data: cf.data.pre Makefile
	@sed \
	-e "s%[@]DEFAULT_HTTP_PORT[@]%$(DEFAULT_HTTP_PORT)%g" \
	-e "s%[@]DEFAULT_ICP_PORT[@]%$(DEFAULT_ICP_PORT)%g" \
	-e "s%[@]DEFAULT_CACHE_EFFECTIVE_USER[@]%$(CACHE_EFFECTIVE_USER)%g" \
	-e "s%[@]DEFAULT_MIME_TABLE[@]%$(DEFAULT_MIME_TABLE)%g" \
	-e "s%[@]DEFAULT_DNSSERVER[@]%$(DEFAULT_DNSSERVER)%g" \
	-e "s%[@]DEFAULT_SSL_CRTD[@]%$(DEFAULT_SSL_CRTD)%g" \
	-e "s%[@]DEFAULT_UNLINKD[@]%$(DEFAULT_UNLINKD)%g" \
	-e "s%[@]DEFAULT_PINGER[@]%$(DEFAULT_PINGER)%g" \
	-e "s%[@]DEFAULT_DISKD[@]%$(DEFAULT_DISKD)%g" \
	-e "s%[@]DEFAULT_LOGFILED[@]%$(DEFAULT_LOGFILED)%g;" \
	-e "s%[@]DEFAULT_CACHE_LOG[@]%$(DEFAULT_CACHE_LOG)%g" \
	-e "s%[@]DEFAULT_ACCESS_LOG[@]%$(DEFAULT_ACCESS_LOG)%g" \
	-e "s%[@]DEFAULT_STORE_LOG[@]%$(DEFAULT_STORE_LOG)%g" \
	-e "s%[@]DEFAULT_PID_FILE[@]%$(DEFAULT_PID_FILE)%g" \
	-e "s%[@]DEFAULT_NETDB_FILE[@]%$(DEFAULT_NETDB_FILE)%g" \
	-e "s%[@]DEFAULT_SWAP_DIR[@]%$(DEFAULT_SWAP_DIR)%g" \
	-e "s%[@]DEFAULT_SSL_DB_DIR[@]%$(DEFAULT_SSL_DB_DIR)%g" \
	-e "s%[@]DEFAULT_ICON_DIR[@]%$(DEFAULT_ICON_DIR)%g" \
	-e "s%[@]DEFAULT_CONFIG_DIR[@]%$(DEFAULT_CONFIG_DIR)%g" \
	-e "s%[@]DEFAULT_ERROR_DIR[@]%$(DEFAULT_ERROR_DIR)%g" \
	-e "s%[@]DEFAULT_PREFIX[@]%$(DEFAULT_PREFIX)%g" \
	-e "s%[@]DEFAULT_HOSTS[@]%$(DEFAULT_HOSTS)%g" \
	-e "s%[@]SQUID[@]%SQUID\ $(VERSION)%g" \
	< $(srcdir)/cf.data.pre >$@

all: cf_parser.cci
	@echo "$<"; # cf.data
	@echo "$@"; # all
