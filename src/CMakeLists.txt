cmake_minimum_required(VERSION 3.9)

set(CMAKE_CXX_STANDARD 11)

list(APPEND CMAKE_MODULE_PATH ../cmake_modules)

find_package(Libevent REQUIRED)

set(CMAKE_CXX_FLAGS "-DBOOST_SP_USE_STD_ATOMIC -DBOOST_AC_USE_STD_ATOMIC")

find_package(OpenSSL REQUIRED)

# -lboost_system -lboost_filesystem -lboost_program_options -lboost_thread -lboost_chrono 
find_package(Boost REQUIRED COMPONENTS system filesystem program_options thread chrono)

find_package(Threads REQUIRED)

find_package(Qt5 COMPONENTS Widgets)

find_package(BerkeleyDB REQUIRED)

include_directories(
  .
  config 
  univalue/include
  secp256k1
  secp256k1/include
  leveldb
  )

add_library(univalue
  univalue/lib/univalue.cpp
	univalue/lib/univalue_read.cpp
	univalue/lib/univalue_write.cpp
  )

add_library(bitcoin_common
  base58.cpp
  chainparams.cpp
  coins.cpp
  compressor.cpp
  core_read.cpp
  core_write.cpp
  key.cpp
  keystore.cpp
  netaddress.cpp
  netbase.cpp
  policy/feerate.cpp
  protocol.cpp
  scheduler.cpp
  script/sign.cpp
  script/standard.cpp
  warnings.cpp
  )

add_library(bitcoin_util
  support/lockedpool.cpp
  chainparamsbase.cpp
  clientversion.cpp
  compat/glibc_sanity.cpp
  compat/glibcxx_sanity.cpp
  compat/strnlen.cpp
  fs.cpp
  random.cpp
  rpc/protocol.cpp
  support/cleanse.cpp
  sync.cpp
  threadinterrupt.cpp
  util.cpp
  utilmoneystr.cpp
  utilstrencodings.cpp
  utiltime.cpp
  )

add_library(bitcoin_cli
  rpc/client.cpp
  )

add_library(bitcoin_crypto
  crypto/aes.cpp
  crypto/chacha20.cpp
  crypto/hmac_sha256.cpp
  crypto/hmac_sha512.cpp
  crypto/ripemd160.cpp
  crypto/scrypt.cpp
  crypto/scrypt-sse2.cpp
  crypto/sha1.cpp
  crypto/sha256.cpp
  crypto/sha512.cpp
  )

add_library(bitcoin_server
  addrdb.cpp
  addrman.cpp
  bloom.cpp
  blockencodings.cpp
  chain.cpp
  checkpoints.cpp
  consensus/tx_verify.cpp
  httprpc.cpp
  httpserver.cpp
  init.cpp
  dbwrapper.cpp
  merkleblock.cpp
  miner.cpp
  net.cpp
  net_processing.cpp
  noui.cpp
  policy/fees.cpp
  policy/policy.cpp
  policy/rbf.cpp
  pow.cpp
  rest.cpp
  rpc/blockchain.cpp
  rpc/mining.cpp
  rpc/misc.cpp
  rpc/net.cpp
  rpc/rawtransaction.cpp
  rpc/server.cpp
  script/sigcache.cpp
  script/ismine.cpp
  timedata.cpp
  torcontrol.cpp
  txdb.cpp
  txmempool.cpp
  ui_interface.cpp
  validation.cpp
  validationinterface.cpp
  versionbits.cpp
  )

add_library(bitcoin_wallet
  wallet/crypter.cpp
  wallet/db.cpp
  wallet/feebumper.cpp
  wallet/rpcdump.cpp
  wallet/rpcwallet.cpp
  wallet/wallet.cpp
  wallet/walletdb.cpp
  )

add_library(bitcoin_zmq
  zmq/zmqabstractnotifier.cpp
  zmq/zmqnotificationinterface.cpp
  zmq/zmqpublishnotifier.cpp
  )

add_library(bitcoin_consensus
  arith_uint256.cpp
  consensus/merkle.cpp
  hash.cpp
  primitives/block.cpp
  primitives/transaction.cpp
  pubkey.cpp
  script/bitcoinconsensus.cpp
  script/interpreter.cpp
  script/script.cpp
  script/script_error.cpp
  uint256.cpp
  utilstrencodings.cpp
  )

add_library(leveldb
  leveldb/db/builder.cc
  leveldb/db/c.cc
  leveldb/db/dbformat.cc
  leveldb/db/db_impl.cc
  leveldb/db/db_iter.cc
  leveldb/db/dumpfile.cc
  leveldb/db/filename.cc
  leveldb/db/log_reader.cc
  leveldb/db/log_writer.cc
  leveldb/db/memtable.cc
  leveldb/db/repair.cc
  leveldb/db/table_cache.cc
  leveldb/db/version_edit.cc
  leveldb/db/version_set.cc
  leveldb/db/write_batch.cc
  leveldb/table/block_builder.cc
  leveldb/table/block.cc
  leveldb/table/filter_block.cc
  leveldb/table/format.cc
  leveldb/table/iterator.cc
  leveldb/table/merger.cc
  leveldb/table/table_builder.cc
  leveldb/table/table.cc
  leveldb/table/two_level_iterator.cc
  leveldb/util/arena.cc
  leveldb/util/bloom.cc
  leveldb/util/cache.cc
  leveldb/util/coding.cc
  leveldb/util/comparator.cc
  leveldb/util/crc32c.cc
  leveldb/util/env.cc
  leveldb/util/env_posix.cc
  leveldb/util/filter_policy.cc
  leveldb/util/hash.cc
  leveldb/util/histogram.cc
  leveldb/util/logging.cc
  leveldb/util/options.cc
  leveldb/util/status.cc
  leveldb/port/port_posix.cc
  )

set(LEVELDB_CXX -DOS_LINUX -DLEVELDB_ATOMIC_PRESENT) 

target_compile_options(leveldb PRIVATE
  ${LEVELDB_CXX}
  )

add_library(leveldb_sse42
  leveldb/port/port_posix_sse.cc
  )

target_compile_options(leveldb_sse42 PRIVATE
  ${LEVELDB_CXX}
  -DLEVELDB_PLATFORM_POSIX_SSE
  )

add_library(memenv
  leveldb/helpers/memenv/memenv.cc
  )

target_compile_options(memenv PRIVATE
  ${LEVELDB_CXX}
  )

add_library(secp256k1
  secp256k1/src/secp256k1.c
  )

target_compile_options(secp256k1 PRIVATE
  -DSECP256K1_BUILD
  )

set(BITCOIN_QT_BASE_CPP
  qt/bantablemodel.cpp
  qt/bitcoinaddressvalidator.cpp
  qt/bitcoinamountfield.cpp
  qt/bitcoingui.cpp
  qt/bitcoinunits.cpp
  qt/clientmodel.cpp
  qt/csvmodelwriter.cpp
  qt/guiutil.cpp
  qt/intro.cpp
  qt/modaloverlay.cpp
  qt/networkstyle.cpp
  qt/notificator.cpp
  qt/optionsdialog.cpp
  qt/optionsmodel.cpp
  qt/peertablemodel.cpp
  qt/platformstyle.cpp
  qt/qvalidatedlineedit.cpp
  qt/qvaluecombobox.cpp
  qt/rpcconsole.cpp
  qt/splashscreen.cpp
  qt/trafficgraphwidget.cpp
  qt/utilitydialog.cpp
  )

set(BITCOIN_QT_WALLET_CPP
  qt/addressbookpage.cpp
  qt/addresstablemodel.cpp
  qt/askpassphrasedialog.cpp
  qt/coincontroldialog.cpp
  qt/coincontroltreewidget.cpp
  qt/editaddressdialog.cpp
  qt/openuridialog.cpp
  qt/overviewpage.cpp
  qt/paymentrequestplus.cpp
  qt/paymentserver.cpp
  qt/receivecoinsdialog.cpp
  qt/receiverequestdialog.cpp
  qt/recentrequeststablemodel.cpp
  qt/sendcoinsdialog.cpp
  qt/sendcoinsentry.cpp
  qt/signverifymessagedialog.cpp
  qt/transactiondesc.cpp
  qt/transactiondescdialog.cpp
  qt/transactionfilterproxy.cpp
  qt/transactionrecord.cpp
  qt/transactiontablemodel.cpp
  qt/transactionview.cpp
  qt/walletframe.cpp
  qt/walletmodel.cpp
  qt/walletmodeltransaction.cpp
  qt/walletview.cpp
  )

set(QT_MOC_CPP
  qt/moc_addressbookpage.cpp
  qt/moc_addresstablemodel.cpp
  qt/moc_askpassphrasedialog.cpp
  qt/moc_bantablemodel.cpp
  qt/moc_bitcoinaddressvalidator.cpp
  qt/moc_bitcoinamountfield.cpp
  qt/moc_bitcoingui.cpp
  qt/moc_bitcoinunits.cpp
  qt/moc_callback.cpp
  qt/moc_clientmodel.cpp
  qt/moc_coincontroldialog.cpp
  qt/moc_coincontroltreewidget.cpp
  qt/moc_csvmodelwriter.cpp
  qt/moc_editaddressdialog.cpp
  qt/moc_guiutil.cpp
  qt/moc_intro.cpp
  qt/moc_macdockiconhandler.cpp
  qt/moc_macnotificationhandler.cpp
  qt/moc_modaloverlay.cpp
  qt/moc_notificator.cpp
  qt/moc_openuridialog.cpp
  qt/moc_optionsdialog.cpp
  qt/moc_optionsmodel.cpp
  qt/moc_overviewpage.cpp
  qt/moc_peertablemodel.cpp
  qt/moc_paymentserver.cpp
  qt/moc_qvalidatedlineedit.cpp
  qt/moc_qvaluecombobox.cpp
  qt/moc_receivecoinsdialog.cpp
  qt/moc_receiverequestdialog.cpp
  qt/moc_recentrequeststablemodel.cpp
  qt/moc_rpcconsole.cpp
  qt/moc_sendcoinsdialog.cpp
  qt/moc_sendcoinsentry.cpp
  qt/moc_signverifymessagedialog.cpp
  qt/moc_splashscreen.cpp
  qt/moc_trafficgraphwidget.cpp
  qt/moc_transactiondesc.cpp
  qt/moc_transactiondescdialog.cpp
  qt/moc_transactionfilterproxy.cpp
  qt/moc_transactiontablemodel.cpp
  qt/moc_transactionview.cpp
  qt/moc_utilitydialog.cpp
  qt/moc_walletframe.cpp
  qt/moc_walletmodel.cpp
  qt/moc_walletview.cpp
  )

#add_library(bitcoinqt
#  )

add_definitions(-DHAVE_CONFIG_H)

message(Boost_LIBRARIES:${Boost_LIBRARIES})
message(CMAKE_THREAD_LIBS_INIT:${CMAKE_THREAD_LIBS_INIT})
message(DB_FOUND:${DB_FOUND})
message(DB_INCLUDE_DIR:${DB_INCLUDE_DIR})
message(DB_LIBRARIES:${DB_LIBRARIES})
message(DB_LIBRARY:${DB_LIBRARY})

#litecoind
#./src/bitcoind.cpp:int main(int argc, char* argv[])
add_executable(litecoind bitcoind.cpp)

target_link_libraries(litecoind
  bitcoin_serverport.h
  bitcoin_common
  univalue
  bitcoin_util
  bitcoin_wallet
  bitcoin_zmq
  bitcoin_consensus
  bitcoin_crypto
  leveldb
  leveldb_sse42
  memenv
  secp256k1
  ${Boost_LIBRARIES}
  ${DB_LIBRARY}
  ${CMAKE_THREAD_LIBS_INIT}
  )

#litecoin-cli
#./src/bitcoin-cli.cpp:int main(int argc, char* argv[])
add_executable(litecoin-cli bitcoin-cli.cpp)

target_link_libraries(litecoin-cli
  bitcoin_cli
  univalue
  bitcoin_util
  bitcoin_crypto
  ${Boost_LIBRARIES}
  ${OPENSSL_LIBRARIES}
  ${LIBEVENT_LIBRARIES}
  ${CMAKE_THREAD_LIBS_INIT}
  )

#litecoin-tx
#./src/bitcoin-tx.cpp:int main(int argc, char* argv[])
add_executable(litecoin-tx bitcoin-tx.cpp)

target_link_libraries(litecoin-tx
  univalue
  bitcoin_common
  bitcoin_util
  bitcoin_consensus
  bitcoin_crypto
  secp256k1
  ${Boost_LIBRARIES}
  ${OPENSSL_LIBRARIES}
  ${CMAKE_THREAD_LIBS_INIT}
  )

#qt/litecoin-qt
#./src/qt/bitcoin.cpp:int main(int argc, char *argv[])
#add_executable(litecoin-qt
#  bitcoinqt
#  bitcoin_server
#  bitcoin_wallet
#  bitcoin_zmq
#  )

#bench/bench_litecoin
#./src/bench/bench_bitcoin.cpp:main(int argc, char** argv)

#test/test_litecoin_fuzzy
#./src/test/test_bitcoin_fuzzy.cpp:int main(int argc, char **argv)

#qt/test/test_litecoin-qt
#./src/qt/test/test_main.cpp:int main(int argc, char *argv[])

#test/test_litecoin
