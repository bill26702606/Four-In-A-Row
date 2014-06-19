'''
遊戲名稱: 四連星

中文翻譯：劉奕鑫，呂仁園

'''

# Four-In-A-Row (a Connect Four clone)
# By Al Sweigart al@inventwithpython.com
# http://inventwithpython.com/pygame
# Released under a "Simplified BSD" license
# -*- coding: UTF-8 -*-

import random, copy, sys, pygame
from pygame.locals import *
from pygame_tc import *

# 加上這行，可以用中文命名 py game 函數
# 目前可用函數如下，可以進一步擴充。

'''
範圍=  range
隨機產出= random.randint


啟動=     pygame.init
鐘類=     pygame.time.Clock
幕設大小= pygame.display.set_mode
幕設標題= pygame.display.set_caption
字型類=   pygame.font.Font
影像下載= pygame.image.load

轉換平滑尺度= pygame.transform.smoothscale

聲音類=   pygame.mixer.Sound

事件取得= pygame.事件.get
結束=     pygame.quit
離開=     sys.exit
隨機選擇= random.choice
畫方形=   pygame.draw.rect
深層複製= copy.deepcopy


事件取得= pygame.事件.get
幕更新=   pygame.display.update

系統離開= sys.exit

事件張貼= pygame.事件.post

方塊類=   pygame.Rect

時間等待= pygame.time.wait
'''



橫軸空間數 = 7  # how many spaces wide the board is
縱軸空間數 = 6 	# how many spaces tall the board is
assert 橫軸空間數 >= 4 and 縱軸空間數 >= 4, 'Board must be at least 4x4.'

最小移動數 = 2 # how many moves to look ahead. (>2 is usually too much)

空間大小 = 50 # size of the tokens and individual board spaces in pixels

畫面更新率 = 30 # frames per second to update the screen
視窗寬度 = 640 # width of the program's window, in pixels
視窗高度 = 480 # height in pixels

橫軸邊 = int((視窗寬度 - 橫軸空間數 * 空間大小) / 2)
縱軸邊 = int((視窗高度 - 縱軸空間數 * 空間大小) / 2)

亮藍色 = (0, 50, 255)
白色 = (255, 255, 255)

背景顏色 = 亮藍色
文字顏色 = 白色

紅 = 'red'
黑 = 'black'
空 = None
使用者 = 'human'
電腦玩家 = 'computer'


def 主函式():
    global 畫面更新率時鐘, 產出面, 紅矩形, 黑矩形, 紅象徵
    global 黑象徵, 板子圖片, 箭頭圖片, 箭頭形狀, 使用者獲勝圖片
    global 電腦玩家獲勝圖片, 獲勝圖片形狀, 連結獲勝圖片

    啟動()
    畫面更新率時鐘 =  鐘類()
    產出面 = 幕設大小((視窗寬度, 視窗高度))
    幕設標題('Four in a Row')

    紅矩形 = 方塊類(int(空間大小 / 2), 視窗高度 - int(3 * 空間大小 / 2), 空間大小, 空間大小)
    黑矩形 = 方塊類(視窗寬度 - int(3 * 空間大小 / 2), 視窗高度 - int(3 * 空間大小 / 2), 空間大小, 空間大小)
    紅象徵 = 影像下載('4row_red.png')
    紅象徵 = 轉換平滑尺度(紅象徵, (空間大小, 空間大小))
    黑象徵 = 影像下載('4row_black.png')
    黑象徵 = 轉換平滑尺度(黑象徵, (空間大小, 空間大小))
    板子圖片 = 影像下載('4row_board.png')
    板子圖片 = 轉換平滑尺度(板子圖片, (空間大小, 空間大小))

    使用者獲勝圖片 = 影像下載('4row_humanwinner.png')
    電腦玩家獲勝圖片 = 影像下載('4row_computerwinner.png')
    連結獲勝圖片 = 影像下載('4row_tie.png')
    獲勝圖片形狀 = 使用者獲勝圖片.get_rect()
    獲勝圖片形狀.center = (int(視窗寬度 / 2), int(視窗高度 / 2))

    箭頭圖片 = 影像下載('4row_arrow.png')
    箭頭形狀 = 箭頭圖片.get_rect()
    箭頭形狀.left = 紅矩形.right + 10
    箭頭形狀.centery = 紅矩形.centery

    判斷是否第一次遊戲 = True

    while True:
        執行遊戲(判斷是否第一次遊戲)
        判斷是否第一次遊戲 = False


def 執行遊戲(判斷是否第一次遊戲):
    if 判斷是否第一次遊戲:
        # Let the computer go first on the first game, so the player
        # can see how the tokens are dragged from the token piles.
        轉換 = 電腦玩家
        顯示幫忙 = True
    else:
        # Randomly choose who goes first.
        if 隨機產出(0, 1) == 0:
            轉換 = 電腦玩家
        else:
            轉換 = 使用者
        顯示幫忙 = False

    # Set up a blank board data structure.

    主板面 = 取得新的版面()

    while True: # main game loop
        if 轉換 == 使用者:
            # Human player's turn.
            取得使用者移動(主板面, 顯示幫忙)
            if 顯示幫忙:
                # turn off help arrow after the first move
                顯示幫忙 = False
            if 判斷獲勝者(主板面, 紅):
                勝者圖片 = 使用者獲勝圖片
                break
            轉換 = 電腦玩家 # switch to other player's turn
        else:
            # Computer player's turn.
            縱行 = 取得電腦移動(主板面)
            電腦移動動畫(主板面, 縱行)
            製作移動(主板面, 黑, 縱行)
            if 判斷獲勝者(主板面, 黑):
                勝者圖片 = 電腦玩家獲勝圖片
                break
            轉換 = 使用者 # switch to other player's turn

        if 版面全滿(主板面):
            # A completely filled board means it's a tie.
            勝者圖片 = 連結獲勝圖片
            break

    while True:
        # Keep looping until player clicks the mouse or quits.
        輸出版面(主板面)
        產出面.blit(勝者圖片, 獲勝圖片形狀)
        幕更新()
        畫面更新率時鐘.tick()
        for 事件 in 事件取得(): # 事件 handling loop
            if 事件.type == QUIT or (事件.type == KEYUP and 事件.key == K_ESCAPE):
                結束()
                離開()
            elif 事件.type == MOUSEBUTTONUP:
                return


def 製作移動(版面, 玩家, 縱行):
    最低的 = 取得最低空格空間(版面, 縱行)
    if 最低的 != -1:
        版面[縱行][最低的] = 玩家


def 輸出版面(版面, 額外標記=None):
    產出面.fill(背景顏色)

    # draw tokens
    空間圖形 = 方塊類(0, 0, 空間大小, 空間大小)
    for x in 範圍(橫軸空間數):
        for y in 範圍(縱軸空間數):
            空間圖形.topleft = (橫軸邊 + (x * 空間大小), 縱軸邊 + (y * 空間大小))
            if 版面[x][y] == 紅:
                產出面.blit(紅象徵, 空間圖形)
            elif 版面[x][y] == 黑:
                產出面.blit(黑象徵, 空間圖形)

    # draw the extra token
    if 額外標記 != None:
        if 額外標記['color'] == 紅:
            產出面.blit(紅象徵, (額外標記['x'], 額外標記['y'], 空間大小, 空間大小))
        elif 額外標記['color'] == 黑:
            產出面.blit(黑象徵, (額外標記['x'], 額外標記['y'], 空間大小, 空間大小))

    # draw board over the tokens
    for x in 範圍(橫軸空間數):
        for y in 範圍(縱軸空間數):
            空間圖形.topleft = (橫軸邊 + (x * 空間大小), 縱軸邊 + (y * 空間大小))
            產出面.blit(板子圖片, 空間圖形)

    # draw the red and black tokens off to the side
    產出面.blit(紅象徵, 紅矩形) # red on the left
    產出面.blit(黑象徵, 黑矩形) # black on the right


def 取得新的版面():
    版面 = []
    for x in 範圍(橫軸空間數):
        版面.append([空] * 縱軸空間數)
    return 版面


def 取得使用者移動(版面, 判斷為第一次移動):
    拖動標記 = False
    標記X, 標記Y = None, None
    while True:
        for 事件 in 事件取得(): # 事件 handling loop
            if 事件.type == QUIT:
                結束()
                離開()
            elif 事件.type == MOUSEBUTTONDOWN and not 拖動標記 and 紅矩形.collidepoint(事件.pos):
                # start of dragging on red token pile.
                拖動標記 = True
                標記X, 標記Y = 事件.pos
            elif 事件.type == MOUSEMOTION and 拖動標記:
                # update the position of the red token being dragged
                標記X, 標記Y = 事件.pos
            elif 事件.type == MOUSEBUTTONUP and 拖動標記:
                # let go of the token being dragged
                if 標記Y < 縱軸邊 and 標記X > 橫軸邊 and 標記X < 視窗寬度 - 橫軸邊:
                    # let go at the top of the screen.
                    縱行 = int((標記X - 橫軸邊) / 空間大小)
                    if 判斷是否為有效移動(版面, 縱行):
                        計算下降標記(版面, 縱行, 紅)
                        版面[縱行][取得最低空格空間(版面, 縱行)] = 紅
                        輸出版面(版面)
                        幕更新()
                        return
                標記X, 標記Y = None, None
                拖動標記 = False
        if 標記X != None and 標記Y != None:
            輸出版面(版面, {'x':標記X - int(空間大小 / 2), 'y':標記Y - int(空間大小 / 2), 'color':紅})
        else:
            輸出版面(版面)

        if 判斷為第一次移動:
            # Show the help arrow for the player's first move.
            產出面.blit(箭頭圖片, 箭頭形狀)

        幕更新()
        畫面更新率時鐘.tick()


def 計算下降標記(版面, 縱行, 顏色):
    x = 橫軸邊 + 縱行 * 空間大小
    y = 縱軸邊 - 空間大小
    下降速度 = 1.0

    最低空格空間 = 取得最低空格空間(版面, 縱行)

    while True:
        y += int(下降速度)
        下降速度 += 0.5
        if int((y - 縱軸邊) / 空間大小) >= 最低空格空間:
            return
        輸出版面(版面, {'x':x, 'y':y, 'color':顏色})
        幕更新()
        畫面更新率時鐘.tick()


def 電腦移動動畫(版面, 縱行):
    x = 黑矩形.left
    y = 黑矩形.top
    速度 = 1.0
    # moving the black tile up
    while y > (縱軸邊 - 空間大小):
        y -= int(速度)
        速度 += 0.5
        輸出版面(版面, {'x':x, 'y':y, 'color':黑})
        幕更新()
        畫面更新率時鐘.tick()
    # moving the black tile over
    y = 縱軸邊 - 空間大小
    速度 = 1.0
    while x > (橫軸邊 + 縱行 * 空間大小):
        x -= int(速度)
        速度 += 0.5
        輸出版面(版面, {'x':x, 'y':y, 'color':黑})
        幕更新()
        畫面更新率時鐘.tick()
    # dropping the black tile
    計算下降標記(版面, 縱行, 黑)


def 取得電腦移動(版面):
    潛在移動數 = 取得潛在移動數(版面, 黑, 最小移動數)
    # get the best fitness from the potential moves
    最好的移動 = -1
    for i in 範圍(橫軸空間數):
        if 潛在移動數[i] > 最好的移動 and 判斷是否為有效移動(版面, i):
            最好的移動 = 潛在移動數[i]
    # find all potential moves that have this best fitness
    最好的移動步數 = []
    for i in 範圍(len(潛在移動數)):
        if 潛在移動數[i] == 最好的移動 and 判斷是否為有效移動(版面, i):
            最好的移動步數.append(i)
    return 隨機選擇(最好的移動步數)


def 取得潛在移動數(版面, 填滿的空格, 預測):
    if 預測 == 0 or 版面全滿(版面):
        return [0] * 橫軸空間數

    if 填滿的空格 == 紅:
        敵人空格 = 黑
    else:
        敵人空格 = 紅

    # Figure out the best move to make.
    潛在移動數 = [0] * 橫軸空間數
    for 第一次移動 in 範圍(橫軸空間數):
        愚弄版面 = 深層複製(版面)
        if not 判斷是否為有效移動(愚弄版面, 第一次移動):
            continue
        製作移動(愚弄版面, 填滿的空格, 第一次移動)
        if 判斷獲勝者(愚弄版面, 填滿的空格):
            # a winning move automatically gets a perfect fitness
            潛在移動數[第一次移動] = 1
            break # don't bother calculating other moves
        else:
            # do other player's counter moves and determine best one
            if 版面全滿(愚弄版面):
                潛在移動數[第一次移動] = 0
            else:
                for 計算移動 in 範圍(橫軸空間數):
                    愚弄版面2 = 深層複製(愚弄版面)
                    if not 判斷是否為有效移動(愚弄版面2, 計算移動):
                        continue
                    製作移動(愚弄版面2, 敵人空格, 計算移動)
                    if 判斷獲勝者(愚弄版面2, 敵人空格):
                        # a losing move automatically gets the worst fitness
                        潛在移動數[第一次移動] = -1
                        break
                    else:
                        # do the recursive call to getPotentialMoves()
                        結果 = 取得潛在移動數(愚弄版面2, 填滿的空格, 預測 - 1)
                        潛在移動數[第一次移動] += (sum(結果) / 橫軸空間數) / 橫軸空間數
    return 潛在移動數


def 取得最低空格空間(版面, 縱行):
    # Return the row number of the lowest empty row in the given 縱行.
    for y in 範圍(縱軸空間數-1, -1, -1):
        if 版面[縱行][y] == 空:
            return y
    return -1


def 判斷是否為有效移動(版面, 縱行):
    # Returns True if there is an empty space in the given 縱行.
    # Otherwise returns False.
    if 縱行 < 0 or 縱行 >= (橫軸空間數) or 版面[縱行][0] != 空:
        return False
    return True


def 版面全滿(版面):
    # Returns True if there are no empty spaces anywhere on the board.
    for x in 範圍(橫軸空間數):
        for y in 範圍(縱軸空間數):
            if 版面[x][y] == 空:
                return False
    return True


def 判斷獲勝者(版面, 填滿的空格):
    # check horizontal spaces
    for x in 範圍(橫軸空間數 - 3):
        for y in 範圍(縱軸空間數):
            if 版面[x][y] == 填滿的空格 and 版面[x+1][y] == 填滿的空格 and 版面[x+2][y] == 填滿的空格 and 版面[x+3][y] == 填滿的空格:
                return True
    # check vertical spaces
    for x in 範圍(橫軸空間數):
        for y in 範圍(縱軸空間數 - 3):
            if 版面[x][y] == 填滿的空格 and 版面[x][y+1] == 填滿的空格 and 版面[x][y+2] == 填滿的空格 and 版面[x][y+3] == 填滿的空格:
                return True
    # check / diagonal spaces
    for x in 範圍(橫軸空間數 - 3):
        for y in 範圍(3, 縱軸空間數):
            if 版面[x][y] == 填滿的空格 and 版面[x+1][y-1] == 填滿的空格 and 版面[x+2][y-2] == 填滿的空格 and 版面[x+3][y-3] == 填滿的空格:
                return True
    # check \ diagonal spaces
    for x in 範圍(橫軸空間數 - 3):
        for y in 範圍(縱軸空間數 - 3):
            if 版面[x][y] == 填滿的空格 and 版面[x+1][y+1] == 填滿的空格 and 版面[x+2][y+2] == 填滿的空格 and 版面[x+3][y+3] == 填滿的空格:
                return True
    return False


if __name__ == '__main__':
    主函式()



