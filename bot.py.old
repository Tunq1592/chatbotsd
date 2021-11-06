from botbuilder.core import TurnContext,ActivityHandler,ConversationState,MessageFactory
from botbuilder.dialogs import DialogSet,WaterfallDialog,WaterfallStepContext,DialogTurnResult
from botbuilder.dialogs.prompts import ChoicePrompt,PromptOptions
from botbuilder.dialogs.choices import Choice
from typing import List, Union

class BotDialog(ActivityHandler):
    def __init__(self,conversation:ConversationState):
        self.con_statea = conversation
        self.state_prop = self.con_statea.create_property("dialog_set")
        self.dialog_set = DialogSet(self.state_prop)
        self.dialog_set.add(ChoicePrompt(ChoicePrompt.__name__))
        self.dialog_set.add(WaterfallDialog("main_dialog",[self.DisplayChoiceList,self.ReadResult,self.account_step,self.account_result]))

    async def DisplayChoiceList(self, step_context: WaterfallStepContext) -> DialogTurnResult:
        listofchoice = [Choice("I need support my account - Tôi cần hỗ trợ về vấn đề tài khoản"),
                        Choice("I need support about system process - Tôi cần hỗ trợ tư vấn về qui trình hệ thống"),
                        Choice("I need to know about status of my ticket in JiraSD - Tôi cần biết thông tin về  ticket tôi đã tạo trên JiraSD"),
                        Choice("Need to talk with IT technician - Tôi cần nói chuyện trực tiếp với bộ phận kĩ thuật")]
        return await step_context.prompt((ChoicePrompt.__name__),
        PromptOptions(prompt=MessageFactory.text("Welcome to IT Help Center. Please choose subject you need assist - Chào mừng bạn đến trung tâm hõ trợ của IT.Vui lòng chọn chủ đề bạn cần hỗ trợ") ,choices=listofchoice))

    async def ReadResult(self, step_context: WaterfallStepContext) -> DisplayChoiceList:
        while True:
            choiceoption = int(step_context._turn_context.activity.text)
            if choiceoption == 1:
                account = [Choice("I need support my HCG account - Tôi cần hỗ trợ về tài khoản HCG"),
                           Choice("I need support my HOSEL account - Tôi cần hỗ trợ về tài khoản HOSEL"),
                           Choice("back to previous category - quay lại danh mục trước")]
                return await step_context.prompt((ChoicePrompt.__name__),
                PromptOptions(prompt=MessageFactory.text("What kind of account you need our support? - Bạn cần hỗ trợ loại tài khoản nào?"),choices=account))
            elif choiceoption == 2:
                await step_context._turn_context.send_activity(MessageFactory.text("Bạn cần hỗ trợ tư vấn về qui trình nào ? "))
            elif choiceoption == 3:
                await step_context._turn_context.send_activity(MessageFactory.text("Vui lòng nhập số ticket cần kiểm tra"))
            elif choiceoption == 4:
                await step_context._turn_context.send_activity(MessageFactory.text("vui lòng để lại lời nhắn cho nhân viên kỹ thuật"))
            await step_context._turn_context.send_activity(MessageFactory.text("press any key to return to previous category - nhấn phím bất kỳ quay lại danh mục trước"))
            return await step_context.end_dialog()
    async def account_step(self, step_context: WaterfallStepContext) -> ReadResult:
        option = int(step_context._turn_context.activity.text)
        if option == 1:
            Hcg = [Choice("My password has expired I need suppor to reset password - Mật khẩu bị hết hạn, tôi cần hỗ trợ đặt lại mật khẩu?"),
                   Choice("I can't login to IDM by my HCG account - Tôi không thể truy cập vào IDM bằng tài khoản HCG"),
                   Choice("Others - Vấn đề khác"),
                   Choice("End this sesion - Kết thúc cuộc trò chuyện")]
            return await step_context.prompt((ChoicePrompt.__name__),PromptOptions(prompt=MessageFactory.text("Please enter your choice of numbers - vui lòng nhập lựa chọn số :"),choices=Hcg))
        if option == 2:
            Hosel = [Choice("My HOSEL account has been lock due to wrong username - password Tài khoản HOSEL của tôi bị khóa do nhập sai tên tài khoản/ mật khẩu"),
                     Choice("My HOSEL account password has been expired - Mật khẩu tài khoản HOSEL của tôi bị hết hạn"),
                     Choice("I can't change HOSEL account password - Tôi không thể tự đổi mật khẩu tài khoản HOSEL"),
                     Choice("Others - Vấn đề khác"),
                     Choice("End this sesion - Kết thúc cuộc trò chuyện")]
            return await step_context.prompt((ChoicePrompt.__name__), PromptOptions(prompt=MessageFactory.text("Please enter your choice of numbers\ vui lòng nhập lựa chọn số :"),choices=Hosel))
        if option == 3:
            await step_context._turn_context.send_activity(MessageFactory.text("press any key to return to previous category - nhấn phím bất kỳ quay lại danh mục trước"))
            return await step_context.end_dialog()
    async def account_result(self, step_context: WaterfallStepContext):
        while True:
            step_context.values["result"] = step_context.result.value
            if "My password has expired I need suppor to reset password - Mật khẩu bị hết hạn, tôi cần hỗ trợ đặt lại mật khẩu?" in step_context.values["result"]:
                await step_context._turn_context.send_activity(MessageFactory.text("Please follow the instruction below to reset your HCG password - Vui lòng làm theo hướng dẫn trong đường link bên dưới để đặt lại mật khẩu : https://homecreditgroup.sharepoint.com/sites/HCVNITSelfServicePortal2/SitePages/Account-Management.aspx#%C4%91%E1%BB%95i-password-hcg-b%E1%BA%B1ng-idm"))
            elif "I can't login to IDM by my HCG account - Tôi không thể truy cập vào IDM bằng tài khoản HCG" in step_context.values["result"]:
                await step_context._turn_context.send_activity(MessageFactory.text("If you believe there is an issue, please access to Jira ServiceDesk portal to 'Report an  issue' - Nếu bạn nghĩ rằng có lỗi hệ thống xảy ra, vui lòng truy cập vào trang Jira ServiceDesk portal để ' Báo lỗi sự cố' : https://servicedesk.homecredit.net/vn"))
            elif "My HOSEL account has been lock due to wrong username - password Tài khoản HOSEL của tôi bị khóa do nhập sai tên tài khoản/ mật khẩu" in  step_context.values["result"]:
                await step_context._turn_context.send_activity(MessageFactory.text("The account will be unlock  after 30mins automatically. Please wait and try later - Tài khoản sẽ tự động được mở lại sau 30 phút. Vui lòng chờ và thử lại sau"))
            elif "My HOSEL account password has been expired - Mật khẩu tài khoản HOSEL của tôi bị hết hạn" in step_context.values["result"]:
                await step_context._turn_context.send_activity(MessageFactory.text("Please follow the instruction below to reset your HOSEL password - Vui lòng làm theo hướng dẫn trong đường link bên dưới để đặt lại mật khẩu https://homecreditgroup.sharepoint.com/sites/HCVNITSelfServicePortal2/SitePages/Account-Management.aspx#c%C3%A1ch-%C4%91%E1%BB%95i-password-ldap"))
            elif "I can't change HOSEL account password - Tôi không thể tự đổi mật khẩu tài khoản HOSEL" in step_context.values["result"]:
                await step_context._turn_context.send_activity(MessageFactory.text("If you believe there is an issue, please access to Jira ServiceDesk portal to 'Report an  issue' - Nếu bạn nghĩ rằng có lỗi hệ thống xảy ra, vui lòng truy cập vào trang Jira ServiceDesk portal để ' Báo lỗi sự cố' : https://servicedesk.homecredit.net/vn"))
            elif "Others - Vấn đề khác" in step_context.values["result"]:
                await step_context._turn_context.send_activity(MessageFactory.text("Please wait a few minutes, our technician will join this conversation to help you - Vui lòng chờ, bộ phận kĩ thuật sẽ tham gia cuộc trò chuyện để hỗ trợ bạn"))
            elif "End this sesion - Kết thúc cuộc trò chuyện" in step_context.values["result"]:
                await step_context._turn_context.send_activity(MessageFactory.text("Thanks for contacting us. Come back any times if you need any support from IT - Cám ơn bạn đã liên hệ. Hãy quay lại bất cứ lúc nào cần IT hỗ trợ nhé."))
            await step_context._turn_context.send_activity(MessageFactory.text("press any key to return to category - nhấn phím bất kỳ để trở lại danh mục"))
            return await step_context.end_dialog()
    async def on_turn(self,turn_context:TurnContext):
        dialog_context = await self.dialog_set.create_context(turn_context)

        if(dialog_context.active_dialog is not None):
            await dialog_context.continue_dialog()
        else:
            await dialog_context.begin_dialog("main_dialog")

        await self.con_statea.save_changes(turn_context)

