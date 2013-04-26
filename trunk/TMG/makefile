#
# Quake2 gamei386.so Makefile for Linux 2.0
#
# Jan '98 by Zoid <zoid@idsoftware.com>
#
# ELF only
#
# Probably requires GNU make
#
# This builds the gamei386.so for Linux based on the q2source_12_11.zip
# release.  
# Put his Makefile in the game subdirectory you get when you unzip
# q2source_12_11.zip.
#
# There are two compiler errors you'll get, the following fixes
# are necessary:
#
# In g_local.h (around line 828), you must change the 
#    typedef struct g_client_s { ... } gclient_t;
# to just:
#    struct g_client_s { ... };
# The typedef is already defined elsewhere (seems to compile fine under
# MSCV++ for Win32 for some reason).
#
# m_player.h has a Ctrl-Z at the end (damn DOS editors).  Remove it or
# gcc complains.
#
# Note that the source in q2source_12_11.zip is for version 3.05.  To
# get it to run with Linux 3.10, change the following in game.h:
#    #define	GAME_API_VERSION	1
# change it to:
#    #define	GAME_API_VERSION	2

#choose 32 or 64 bit
#must change cflags to m32 or m64
ARCH=i386
#ARCH=x86_64

CC=gcc
BASE_CFLAGS=-Dstricmp=strcasecmp -D_strnicmp=strncasecm -Dstrnicmp=strncasecmp -Dstrcmpi=strcasecmp

#use these cflags to optimize it
CFLAGS=$(BASE_CFLAGS) -m32 -O6 -ffast-math -funroll-loops \
	-fomit-frame-pointer -fexpensive-optimizations -falign-loops=2 \
	-falign-jumps=2 -falign-functions=2 \
	-I/usr/include/mysql -g -pipe -Wp, -fexceptions \
	-fasynchronous-unwind-tables \
	-D_GNU_SOURCE -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -fno-strict-aliasing \
	-fwrapv -fPIC -DUNIV_LINUX -DUNIV_LINUX
#use these when debugging 
#CFLAGS=$(BASE_CFLAGS) -g

LDFLAGS=-ldl -lm
SHLIBEXT=so
SHLIBCFLAGS=-fPIC
SHLIBLDFLAGS=-shared

DO_CC=$(CC) $(CFLAGS) $(mysql_config --cflags) $(SHLIBCFLAGS) -o $@ -c $< -rdynamic -L/usr/lib/mysql -lmysqlclient -lz -lcrypt -lnsl -lm -lssl -lcrypto

#############################################################################
# SETUP AND BUILD
# GAME
#############################################################################

.c.o:
	$(DO_CC)

GAME_OBJS = \
	g_ai.o g_chase.o g_cmds.o g_combat.o g_ctf.o g_func.o g_items.o g_main.o g_misc.o \
	g_monster.o g_phys.o g_save.o g_spawn.o g_svcmds.o g_target.o g_trigger.o g_utils.o \
	g_weapon.o m_move.o p_client.o p_hud.o p_menu.o p_trail.o p_view.o p_weapon.o q_shared.o \
	filehand.o maplist.o timer.o eavy.o entdump.o admin.o mysql.o e_hook.o
game$(ARCH).$(SHLIBEXT) : $(GAME_OBJS)
	$(CC) $(CFLAGS) $(SHLIBLDFLAGS) -o $@ $(GAME_OBJS) -rdynamic -L/usr/lib/mysql -lmysqlclient -lz -lcrypt -lnsl -lm -lssl -lcrypto


#############################################################################
# MISC
#############################################################################

clean:
	-rm -f $(GAME_OBJS) *.so

depend:
	gcc -MM $(GAME_OBJS:.o=.c)

#############################################################################
# DEPENDANCIES
#############################################################################

g_ai.o: g_ai.c g_local.h q_shared.h game.h
p_client.o: p_client.c g_local.h q_shared.h game.h m_player.h
g_cmds.o: g_cmds.c g_local.h q_shared.h game.h m_player.h
g_combat.o: g_combat.c g_local.h q_shared.h game.h
g_func.o: g_func.c g_local.h q_shared.h game.h
g_items.o: g_items.c g_local.h q_shared.h game.h
g_main.o: g_main.c g_local.h q_shared.h game.h
g_misc.o: g_misc.c g_local.h q_shared.h game.h
g_monster.o: g_monster.c g_local.h q_shared.h game.h
g_phys.o: g_phys.c g_local.h q_shared.h game.h
g_save.o: g_save.c g_local.h q_shared.h game.h
g_spawn.o: g_spawn.c g_local.h q_shared.h game.h
g_target.o: g_target.c g_local.h q_shared.h game.h
g_trigger.o: g_trigger.c g_local.h q_shared.h game.h
g_utils.o: g_utils.c g_local.h q_shared.h game.h
g_weapon.o: g_weapon.c g_local.h q_shared.h game.h
m_move.o: m_move.c g_local.h q_shared.h game.h
p_hud.o: p_hud.c g_local.h q_shared.h game.h
p_trail.o: p_trail.c g_local.h q_shared.h game.h
p_view.o: p_view.c g_local.h q_shared.h game.h m_player.h
p_weapon.o: p_weapon.c g_local.h q_shared.h game.h m_player.h
q_shared.o: q_shared.c q_shared.h
filehand.o: filehand.c g_local.h
maplist.o: maplist.c g_local.h
timer.o: timer.c g_local.h maplist.h
eavy.o: eavy.h g_local.h
