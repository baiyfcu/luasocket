include_directories(${LUA_INCLUDE_DIR})

IF(WIN32)
	SET(luasocket_PLATFORM_SPECIFIC
			${LuaSocket_SOURCE_DIR}/src/wsocket.h
			${LuaSocket_SOURCE_DIR}/src/wsocket.c
	)
ELSEIF(UNIX)
	SET(luasocket_PLATFORM_SPECIFIC
			${LuaSocket_SOURCE_DIR}/src/usocket.h
			${LuaSocket_SOURCE_DIR}/src/usocket.c
			${LuaSocket_SOURCE_DIR}/src/unix.h
			${LuaSocket_SOURCE_DIR}/src/unix.c
			${LuaSocket_SOURCE_DIR}/src/unixdgram.h
			${LuaSocket_SOURCE_DIR}/src/unixdgram.c
			${LuaSocket_SOURCE_DIR}/src/unixstream.h
			${LuaSocket_SOURCE_DIR}/src/unixstream.c
			${LuaSocket_SOURCE_DIR}/src/serial.c
	)
ENDIF(WIN32)

SET(luasocket_SOCKET
		${LuaSocket_SOURCE_DIR}/src/auxiliar.h
		${LuaSocket_SOURCE_DIR}/src/auxiliar.c
		${LuaSocket_SOURCE_DIR}/src/buffer.c
		${LuaSocket_SOURCE_DIR}/src/buffer.h
		${LuaSocket_SOURCE_DIR}/src/compat.h
		${LuaSocket_SOURCE_DIR}/src/compat.c
		${LuaSocket_SOURCE_DIR}/src/except.h
		${LuaSocket_SOURCE_DIR}/src/except.c
		${LuaSocket_SOURCE_DIR}/src/inet.h
		${LuaSocket_SOURCE_DIR}/src/inet.c
		${LuaSocket_SOURCE_DIR}/src/io.h
		${LuaSocket_SOURCE_DIR}/src/io.c
		${LuaSocket_SOURCE_DIR}/src/luasocket.c
		${LuaSocket_SOURCE_DIR}/src/mime.h
		${LuaSocket_SOURCE_DIR}/src/mime.c
		${LuaSocket_SOURCE_DIR}/src/options.h
		${LuaSocket_SOURCE_DIR}/src/options.c
		${LuaSocket_SOURCE_DIR}/src/pierror.h
		${LuaSocket_SOURCE_DIR}/src/select.h
		${LuaSocket_SOURCE_DIR}/src/select.c
		${LuaSocket_SOURCE_DIR}/src/socket.h
		${LuaSocket_SOURCE_DIR}/src/tcp.h
		${LuaSocket_SOURCE_DIR}/src/tcp.c
		${LuaSocket_SOURCE_DIR}/src/timeout.h
		${LuaSocket_SOURCE_DIR}/src/timeout.c
		${LuaSocket_SOURCE_DIR}/src/udp.h
		${LuaSocket_SOURCE_DIR}/src/udp.c
		${luasocket_PLATFORM_SPECIFIC}
)

SET(luasocket_PUBLIC_HEADERS
		${LuaSocket_SOURCE_DIR}/src/luasocket.h
)


SET(luasocket_RESOURCE_FILES
		${LuaSocket_SOURCE_DIR}/src/http.lua
		${LuaSocket_SOURCE_DIR}/src/tp.lua
		${LuaSocket_SOURCE_DIR}/src/ftp.lua
		${LuaSocket_SOURCE_DIR}/src/smtp.lua
		${LuaSocket_SOURCE_DIR}/src/url.lua
		${LuaSocket_SOURCE_DIR}/src/mbox.lua
		${LuaSocket_SOURCE_DIR}/src/headers.lua

)

SET(luasocketcommon_RESOURCE_FILES
		${LuaSocket_SOURCE_DIR}/src/ltn12.lua
		${LuaSocket_SOURCE_DIR}/src/socket.lua
		${LuaSocket_SOURCE_DIR}/src/mime.lua
)


ADD_LIBRARY(luasocket_library_static STATIC ${luasocket_PUBLIC_HEADERS} ${luasocket_SOCKET})
SET_TARGET_PROPERTIES(luasocket_library_static PROPERTIES OUTPUT_NAME "luasocket")
SET_TARGET_PROPERTIES(luasocket_library_static PROPERTIES COMPILE_FLAGS "${LUASOCKET_C_FLAGS}")
TARGET_LINK_LIBRARIES(luasocket_library_static ${LUASOCKET_LINK_FLAGS})



INSTALL(TARGETS
		luasocket_library_static
		#	RUNTIME DESTINATION bin
		LIBRARY DESTINATION lib
		ARCHIVE DESTINATION lib
		#	ARCHIVE DESTINATION lib/static
)


INSTALL(FILES ${luasocket_PUBLIC_HEADERS} DESTINATION include)

INSTALL(FILES ${luasocket_RESOURCE_FILES} DESTINATION ${LUAPACKAGE_LDIR}/socket)
INSTALL(FILES ${luasocketcommon_RESOURCE_FILES} DESTINATION ${LUAPACKAGE_LDIR})