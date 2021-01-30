#
#  FILE: CMakeLists.txt
#
#  DESC: stubgen CMake file
#	 modified from O'Reilly's lex & yacc, Second Edition
#
#  Copyright (c) 2004, 2021  Michael J. Radwin

cmake_minimum_required(VERSION 3.1)

project(stubgen)

# Debug settings are on.  Having the SGDEBUG macro defined will make
# generate 'stubgen.log' files on each execution of stubgen, which are
# useful for debugging, but probably annoying.
set(CMAKE_C_COMPILER_FLAGS -g -O2 -DSGDEBUG -Wall -Werror)

# You'll need to use a lexer that supports the exclusive start state
# mechanism (the '%x' directive).  GNU Flex does this just fine.
find_package(FLEX REQUIRED)

# You should be able to use any of the 3 common flavors of yacc:
# AT&T Yacc, Berkeley Yacc, or GNU Bison.  We've tested with bison.
find_package(BISON REQUIRED)

set(SRCS table.c table.h util.c util.h main.c)

flex_target(lexer lexer.l ${CMAKE_CURRENT_BINARY_DIR}/lex.yy.c)
bison_target(parser parser.y ${CMAKE_CURRENT_BINARY_DIR}/y.tab.c COMPILE_FLAGS -y)

add_executable(${PROJECT_NAME} ${SRCS} ${FLEX_lexer_OUTPUTS} ${BISON_parser_OUTPUTS})
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})

# for a lex other than GNU Flex, we must link with the -ll library.
target_link_libraries(${PROJECT_NAME} l)
