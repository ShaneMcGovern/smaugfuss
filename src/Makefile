CC = g++
C_FILES = act_comm.c act_info.c act_move.c act_wiz.c ban.c boards.c \
          build.c calendar.c chess.c clans.c color.c comm.c comments.c const.c db.c deity.c \
          dns.c fight.c handler.c hashstr.c hint.c hotboot.c imm_host.c interp.c \
          liquids.c magic.c makeobjs.c mapout.c mapper.c mccp.c \
          misc.c mpxset.c mssp.c mud_comm.c mud_prog.c news.c planes.c player.c polymorph.c \
          renumber.c reset.c save.c services.c sha256.c shops.c skills.c special.c tables.c \
          track.c update.c variables.c weather.c

W_FLAGS = -Wall -Werror -Wshadow -Wformat-security -Wpointer-arith -Wcast-align -Wredundant-decls

ifdef CYGWIN
    C_FLAGS = -g2 -DCYGWIN $(W_FLAGS) -export-dynamic
    L_FLAGS = -lz -ldl
    TARGET = smaug.exe
    TARGET_CLEAN = *.o smaug.exe dependencies.d resolver.exe resolver.o *~
else
    C_FLAGS = -g2 $(W_FLAGS) -export-dynamic
    L_FLAGS = -lz -ldl
    TARGET = smaug
    TARGET_CLEAN = o/*.o smaug dependencies.d resolver resolver.o *~
endif

ifdef SOLARIS
    C_FLAGS += -Dsun -DSYSV
    L_FLAGS += -lnsl -lsocket
    D_FLAGS = -g2 -O -lnsl -lsocket
else
    D_FLAGS = -g2 -O
endif

ifdef IMC
    C_FLAGS += -DIMC -DIMCSMAUG
    C_FILES := imc.c $(C_FILES)
endif

O_FILES := $(C_FILES:.c=.o)
DEP_FILES := $(C_FILES:.c=.d)

all: smaug dns

-include $(DEP_FILES)

smaug: $(O_FILES)
	rm -f $(TARGET)
	$(CC) -o $(TARGET) $(O_FILES) $(L_FLAGS)
	@echo "Generating dependency file ..."
	@$(CC) -MM $(C_FLAGS) $(C_FILES) > dependencies.d
	@echo "Done compiling mud."
	chmod g+w $(TARGET)
	chmod a+x $(TARGET)
	chmod g+w $(O_FILES)

dns: resolver.o
	rm -f resolver
	$(CC) $(D_FLAGS) -o resolver resolver.o
	@echo "Done compiling DNS resolver."
	chmod g+w resolver
	chmod a+x resolver
	chmod g+w resolver.o

clean:
	rm -f $(TARGET_CLEAN)

%.o: %.c
	echo "  Compiling $@ ..."
	$(CC) -c $(C_FLAGS) $< -o $@

%.d: %.c
	$(CC) -MM $(C_FLAGS) $< > $@

.PHONY: all clean dns smaug
