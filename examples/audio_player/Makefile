############################################################################
# audio_player/Makefile
#
#   Copyright 2018 Sony Semiconductor Solutions Corporation
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in
#    the documentation and/or other materials provided with the
#    distribution.
# 3. Neither the name of Sony Semiconductor Solutions Corporation nor
#    the names of its contributors may be used to endorse or promote
#    products derived from this software without specific prior written
#    permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS
# OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
# AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
############################################################################

-include $(TOPDIR)/.config
-include $(TOPDIR)/Make.defs
-include $(SDKDIR)/Make.defs
include $(APPDIR)/Make.defs

# Audio application info

CONFIG_EXAMPLES_AUDIO_PLAYER_PRIORITY ?= SCHED_PRIORITY_DEFAULT
CONFIG_EXAMPLES_AUDIO_PLAYER_STACKSIZE ?= 2048

APPNAME   = audio_player
PRIORITY  = $(CONFIG_EXAMPLES_AUDIO_PLAYER_PRIORITY)
STACKSIZE = $(CONFIG_EXAMPLES_AUDIO_PLAYER_STACKSIZE)

# Audio Example

MAINSRC = audio_player_main.cxx

# Audio Example paths

AUDIODIR = $(SDKDIR)$(DELIM)modules$(DELIM)audio

# Audio Example flags

ifeq ($(WINTOOL),y)
CXXFLAGS += -I "${shell cygpath -w $(AUDIODIR)}"
ifeq ($(CONFIG_EXAMPLES_AUDIO_PLAYER_USEPOSTPROC),y)
CXXFLAGS += -I "${shell cygpath -w worker/userproc/include}"
endif
else
CXXFLAGS += -I $(AUDIODIR)
ifeq ($(CONFIG_EXAMPLES_AUDIO_PLAYER_USEPOSTPROC),y)
CXXFLAGS += -I worker/userproc/include
endif
endif

CXXFLAGS += -D_POSIX
CXXFLAGS += -DUSE_MEMMGR_FENCE
CXXFLAGS += -DATTENTION_USE_FILENAME_LINE

CONFIG_EXAMPLES_AUDIO_PLAYER_PROGNAME ?= audio_player$(EXEEXT)
PROGNAME = $(CONFIG_EXAMPLES_AUDIO_PLAYER_PROGNAME)

include $(APPDIR)/Application.mk

ifeq ($(CONFIG_EXAMPLES_AUDIO_PLAYER_USEPOSTPROC),y)

build_worker:
	$(Q) $(MAKE) -C worker TOPDIR="$(TOPDIR)" SDKDIR="$(SDKDIR)" APPDIR="$(APPDIR)" CROSSDEV=$(CROSSDEV)

.built: build_worker

clean_worker:
	@$(MAKE) -C worker TOPDIR="$(TOPDIR)" SDKDIR="$(SDKDIR)" APPDIR="$(APPDIR)" CROSSDEV=$(CROSSDEV) clean

clean: clean_worker

endif
